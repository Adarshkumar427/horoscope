<script setup>
import { ref, watch } from 'vue';

const zodiac = ref('');
const items = [
    { value: 'aries', title: 'Aries (March 21 - April 19)' },
    { value: 'taurus', title: 'Taurus (April 20 - May 20)' },
    { value: 'gemini', title: 'Gemini (May 21 - June 20)' },
    { value: 'cancer', title: 'Cancer (June 21 - July 22)' },
    { value: 'leo', title: 'Leo (July 23 - August 22)' },
    { value: 'virgo', title: 'Virgo (August 23 - September 22)' },
    { value: 'libra', title: 'Libra (September 23 - October 22)' },
    { value: 'scorpio', title: 'Scorpio (October 23 - November 21)' },
    { value: 'sagittarius', title: 'Sagittarius (November 22 - December 21)' },
    { value: 'capricorn', title: 'Capricorn (December 22 - January 19)' },
    { value: 'aquarius', title: 'Aquarius (January 20 - February 18)' },
    { value: 'pisces', title: 'Pisces (February 19 - March 20)' },
]

const primary = ref('')
const love = ref('')
const general = ref('')
const career = ref('')
const health = ref('')
const period = ref('today')
const language = ref('english')
const type = ref('overall')
const date = ref()
const loading = ref(false)

async function getHoroscope() {
    if (!zodiac.value) {
        return
    }

    loading.value = true
    const requestBody = {
        zodiac: zodiac.value,
        language: language.value,
        date: date.value,
        period: period.value,
        apiKey: import.meta.env.VITE_API_KEY,
        domainSecretCode: import.meta.env.VITE_DOMAIN_SECRET_KEY
    }

    const res = await fetch('https://horoscope-api.riturajshakti.workers.dev/horoscope', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(requestBody)
    });
    if (!res.ok) {
        alert('Failed to fetch the horoscope')
        return
    }

    const data = await res.json()
    for (let e of data.result) {
        if (e.type === 'primary') {
            primary.value = e.horoscope
        }
        else if (e.type === 'love') {
            love.value = e.horoscope
        } else if (e.type === 'career') {
            career.value = e.horoscope
        } else if (e.type === 'health') {
            health.value = e.horoscope
        } else if (e.type === 'general') {
            general.value = e.horoscope
        }

    }
    loading.value = false
}

watch([zodiac, language, period], getHoroscope);



</script>
<template>

    <v-container>
        <v-card color="deep-purple-accent-3" class="position mb-2">
            <v-nav>
                <h1 class="text-center mt-1 mb-2 text-white">Free Horoscope</h1>
            </v-nav>
        </v-card>

        <v-select class=" mt-16 mb-5" v-model="zodiac" label="zodiac" :items="items" item-title="title"
            item-value="value"></v-select>

        <div class="d-flex justify-space-between align-center flex-wrap manage">
            <div>
                <v-btn-toggle v-model="period" color="deep-purple-accent-3" rounded="0" group>
                    <v-btn value="today">
                        Today
                    </v-btn>

                    <v-btn value="tomorrow">
                        Tomorrow
                    </v-btn>

                    <v-btn value="weekly">
                        Weekly
                    </v-btn>

                    <v-btn value="monthly">
                        Monthly
                    </v-btn>
                </v-btn-toggle>
            </div>

            <div>
                <v-btn-toggle v-model="language" color="deep-purple-accent-3" rounded="0" group class="language-toggle">
                    <v-btn value="english">
                        English
                    </v-btn>

                    <v-btn value="hindi">
                        Hindi
                    </v-btn>
                </v-btn-toggle>
            </div>
        </div>

        <div class="d-flex justify-space-between align-center flex-wrap change">
            <div>
                <v-btn-toggle v-model="period" color="deep-purple-accent-3" rounded="0" group>
                    <v-btn value="today">
                        Today
                    </v-btn>

                    <v-btn value="tomorrow">
                        Tomorr
                    </v-btn>

                    <v-btn value="weekly">
                        Week
                    </v-btn>

                    <v-btn value="monthly">
                        Month
                    </v-btn>
                </v-btn-toggle>
            </div>

            <div>
                <v-btn-toggle v-model="language" color="deep-purple-accent-3" rounded="0" group class="language-toggle">
                    <v-btn value="english">
                        English
                    </v-btn>

                    <v-btn value="hindi">
                        Hindi
                    </v-btn>
                </v-btn-toggle>
            </div>
        </div>

        <v-card class="mt-6">
            <v-skeleton-loader v-if="loading" type="article"></v-skeleton-loader>
            <template v-else>
                <v-tabs v-model="type" align-tabs="start" color="deep-purple-accent-4">
                    <v-tab value="overall">Overall</v-tab>
                    <v-tab value="love">Love</v-tab>
                    <v-tab value="finance">Finance</v-tab>
                    <v-tab value="career">Career</v-tab>
                    <v-tab value="health">Health</v-tab>
                </v-tabs>
                <v-card-text>
                    <v-tabs-window v-model="type" class="text-justify">
                        <v-tabs-window-item value="overall">
                            <v-container fluid>
                                <p>{{ primary }}</p>
                            </v-container>
                        </v-tabs-window-item>
                        <v-tabs-window-item value="love">
                            <v-container fluid>
                                <p>{{ love }}</p>
                            </v-container>
                        </v-tabs-window-item>
                        <v-tabs-window-item value="finance">
                            <v-container fluid>
                                <p>{{ general }}</p>
                            </v-container>
                        </v-tabs-window-item>
                        <v-tabs-window-item value="career">
                            <v-container fluid>
                                <p>{{ career }}</p>
                            </v-container>
                        </v-tabs-window-item>
                        <v-tabs-window-item value="health">
                            <v-container fluid>
                                <p>{{ health }}</p>
                            </v-container>
                        </v-tabs-window-item>
                    </v-tabs-window>
                </v-card-text>
            </template>
        </v-card>

    </v-container>

</template>

<style scoped>
.position {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 10;
    /* Assign a reasonable z-index */
}

.language-toggle {
    margin-left: auto;
}

.change {
    display: none !important;
}

@media (max-width: 650px) {
    .language-toggle {
        margin-top: 16px;
    }

    .manage {
        display: none !important;
    }

    .change {
        display: block !important;
    }
}
</style>