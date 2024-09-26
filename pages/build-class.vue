<script setup lang="ts">
import { bool, object, string, array, number, type InferType } from 'yup'
import type { FormSubmitEvent } from '#ui/types'

const state = reactive({
    className: undefined,
    tagline: undefined,
    description: undefined,
    rac: ref(false),
    primeScores: undefined,
    hitDice: undefined,
    hitDiceCap: undefined,
    maxLevels: undefined,
    armorAllowed: undefined,
    weaponsAllowed: undefined,
    startingEquipment: undefined,

})

type Schema = InferType<typeof schema>

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
    startinEquipment: string()
})

const advancementColumns = [
    {
        label: "Level",
        key: "level"
    }
]
const startLevel = 1
const levels = computed(() => {
    
})
const advancementRows = [{
    level: startLevel
}]
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
                        <UFormGroup description="Is this a kindred or race as class?">
                            <UCheckbox v-model="state.rac" label="K/RAC?" />
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
                    <div class="text-xl uppercase">{{ state.className }}</div>
                    <UDivider />
                    <div><i>{{ state.tagline }}</i></div><br />
                    <p>Prime Abilities: {{ state.primeScores }}</p>
                    <p>Hit Points: {{ state.hitDice }} per Level, +1 after Level {{ state.hitDiceCap }}</p>
                    <p>Armor: {{ state.armorAllowed }}</p>
                    <p>Weapons: {{ state.weaponsAllowed }}</p>
                    <template #footer>
                        <div class="h-8">Footer</div>
                    </template>
                    <UTable :columns="advancementColumns" :rows="advancementRows"></UTable>
                </UCard>
            </UContainer>
        </div>
    </div>
</template>