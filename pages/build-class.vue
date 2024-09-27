<script setup lang="ts">
import { bool, object, string, array, number, type InferType } from 'yup'
import type { FormSubmitEvent } from '#ui/types'

const schema = object({
    className: string().required('Required'),
    description: string(),
    tagline: string(),
    rac: bool(),
    primeScores: array(),
    hitDice: string(),
    hitDiceCap: number(),
    maxLevels: number(),
    armorAllowed: array(),
    weaponsAllowed: array(),
    startingEquipment: string()
})

type Schema = InferType<typeof schema>

const state = reactive({
    className: undefined,
    tagline: undefined,
    description: undefined,
    rac: ref(false),
    primeScores: undefined,
    hitDice: ref('d8'),
    hitDiceCap: ref(9),
    hitPointsThereafer: ref(2),
    maxLevels: ref(15),
    armorAllowed: undefined,
    weaponsAllowed: undefined,
    startingEquipment: undefined,

})




const advancementColumns = [
    {
        label: "Level",
        key: "level",
        class: 'py-0 px-0'
    },
    {
        label: "XP",
        key: "xp",
        class: 'py-0 px-0'
    },
    {
        label: "HD",
        key: "hd",
        class: 'py-0 px-0'
    },
    {
        label: "THAC0",
        key: "thac0",
        class: 'py-0 px-0'
    }
]

function buildAdvancement(baseXp: number, maxLevels: number, doubleUntil: number, floorAt: [number, number], thereafter: number, hd: string, hdCap: number, hpAfter: number): Array<object> {
    const advancementRows: Array<object> = []
    let thisXp = 0
    for (let idx = 1; idx <= maxLevels; idx++) {

        if (idx == 1) {
            thisXp = 0
        } else if (idx == 2) {
            thisXp = baseXp
        } else if (idx == floorAt[0]) {
            thisXp = floorAt[1]
        } else if (idx <= doubleUntil) {
            thisXp *= 2
        } else {
            thisXp += thereafter
        }


        advancementRows.push({
            level: {
                value: idx, class: 'py-0 px-0'
            },
            xp: {
                value: thisXp, class: 'py-0 px-0'
            },
            hd: {
                value: [idx, hd].join(""), class: 'py-0 px-0'
            }
        })
    }
    return advancementRows
}

function numberWithCommas(x: number) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
}

</script>
<template>
    <div class="grid grid-cols-2">
        <div>
            <UContainer>
                <UCard>
                    <UForm :state="state" class="space-y-2" :schema="schema">
                        <!-- Consider using a schema as well -->
                        <UFormGroup label="Class Name" description="The name of the new class." required>
                            <UInput v-model="state.className" placeholder="e.g., Fighter" />
                        </UFormGroup>
                        <UFormGroup description="Is this a race as class?">
                            <UCheckbox v-model="state.rac" label="RAC?" />
                        </UFormGroup>
                        <UFormGroup label="Tagline" description="One sentence summary of the class">
                            <UInput v-model="state.tagline"></UInput>
                        </UFormGroup>
                        <UFormGroup label="Prime Scores" description="Primary ability scores.">
                            <UInput v-model="state.primeScores" />
                        </UFormGroup>
                        <UFormGroup label="Hit Dice" description="Hit dice used to calculate hit points.">
                            <UInput v-model="state.hitDice" placeholder="e.g., 1d8" />
                        </UFormGroup>
                        <UFormGroup label="Hit Dice Cap"
                            description="Level after which the class stops applying the full hit dice.">
                            <UInput v-model="state.hitDiceCap" />
                        </UFormGroup>
                        <UFormGroup label="Max Levels" description="Maximim number of levels.">
                            <UInput v-model="state.maxLevels" />
                        </UFormGroup>
                        <UFormGroup label="Armor Allowed" description="What armor types can the class use?">
                            <UInput v-model="state.armorAllowed" />
                        </UFormGroup>
                        <UFormGroup label="Weapons Allowed" description="What weapon types can the class use?">
                            <UInput v-model="state.weaponsAllowed" />
                        </UFormGroup>
                    </UForm>
                </UCard>
            </UContainer>
        </div>
        <div>
            <UContainer>
                <UCard>
                    <template #header>
                        <div class="h-8">
                            Preview
                        </div>
                    </template>
                    <div class="flex-inline">
                        <span class="text-xl uppercase">{{ state.className }}</span>

                    </div>
                    <UDivider />

                    <div class="mb-2"><i>{{ state.tagline }}</i></div>
                    <div v-if="state.rac">
                        <UBadge size="xs">Race as Class</UBadge>
                    </div>
                    <div class="mt-2"></div>
                    <p>Requirements: None </p>
                    <p>Prime Abilities: {{ state.primeScores }}</p>
                    <p>Hit Points: {{ state.hitDice }} per Level, +1 after Level {{ state.hitDiceCap }}</p>
                    <p>Armor: {{ state.armorAllowed }}</p>
                    <p>Weapons: {{ state.weaponsAllowed }}</p>
                    <template #footer>
                        <div class="h-8">Footer</div>
                    </template>
                    <UTable :columns="advancementColumns"
                        :rows="buildAdvancement(2000, state.maxLevels, 9, [8, 120000], 120000, state.hitDice, state.hitDiceCap, state.hitPointsThereafer)"
                        class="py-2">
                        <template #level-data="{ row }">
                            {{ row.level.value }}
                        </template>
                        <template #xp-data="{ row }">
                            {{ numberWithCommas(row.xp.value) }}
                        </template>
                        <template #hd-data="{ row }">
                            {{ numberWithCommas(row.hd.value) }}
                        </template>
                    </UTable>
                </UCard>
            </UContainer>
        </div>
    </div>
</template>