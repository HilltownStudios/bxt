<script lang="js"></script>
<script setup lang="js">
const state = reactive({
    className: undefined,
    rac: ref(false),
    primeScores: undefined,
    hitDice: undefined,
    hitDiceCap: undefined,
    maxLevels: undefined,
    armorAllowed: undefined,
    weaponsAllowed: undefined,
    startingEquipment: undefined,

})
</script>
<template>
    <UContainer>
        <UCard>
            <UForm state="state" class="space-y-2">
                <!-- Consider using a schema as well -->
                <UFormGroup label="Class Name" description="The name of the new class." required>
                    <UInput v-model="className" placeholder="e.g., Fighter" />
                </UFormGroup>
                <UFormGroup description="Is this a kindred or race as class?">
                    <UCheckbox v-model="rac" label="K/RAC?" />
                </UFormGroup>
                <UFormGroup label="Prime Scores" description="Primary ability scores.">
                    <UInput v-model="primeScores" />
                </UFormGroup>
                <UFormGroup label="Hit Dice" description="Hit dice used to calculate hit points.">
                    <UInput v-model="hitDice" placeholder="e.g., 1d8" />
                </UFormGroup>
                <UFormGroup label="Hit Dice Cap"
                    description="Level after which the class stops applying the full hit dice.">
                    <UInput v-model="hitDiceCap" />
                </UFormGroup>
                <UFormGroup label="Max Levels" description="Maximim number of levels.">
                    <UInput v-model="maxLevels" />
                </UFormGroup>
                <UFormGroup label="Armor Allowed" description="What armor types can the class use?">
                    <UInput v-model="armorAllowed" />
                </UFormGroup>
                <UFormGroup label="Weapons Allowed" description="What weapon types can the class use?">
                    <UInput v-model="weaponsAllowed" />
                </UFormGroup>
            </UForm>
        </UCard>
    </UContainer>
</template>