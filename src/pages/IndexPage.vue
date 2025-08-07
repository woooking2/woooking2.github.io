<template>
  <q-page>
    <div class="q-pa-md q-gutter-sm">
      <div>
        <q-input v-model="text" filled autogrow />
      </div>
      <div class="q-gutter-sm">
        <q-radio
          v-for="rule in rules"
          :key="rule.name"
          v-model="selectedRule"
          :val="rule.name"
          :label="rule.name"
        />
      </div>
      <div>
        <q-input :model-value="output" :readonly="true" filled autogrow />
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { alphabetRule, ruleMap, rules } from 'src/models/rule';
import { computed, ref } from 'vue';

const text = ref<string>('');
const selectedRule = ref<string>(alphabetRule.name);

function translate(input: string, ruleName: string) {
  const texts = input.split(/\s+/);
  const results = [];
  const rule = ruleMap[ruleName];
  if (!rule) {
    return input;
  }
  for (const text of texts) {
    if (rule.dict[text] !== undefined) {
      results.push(rule.dict[text]);
    } else {
      results.push(text);
    }
  }
  return results.join('');
}

const output = computed(() => translate(text.value, selectedRule.value));
</script>
