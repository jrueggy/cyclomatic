<template>
    <v-toolbar color="primary">
        <v-toolbar-title>Cyclomatix complexity calculator</v-toolbar-title>
    </v-toolbar>

    <v-container>
        <v-row>
            <v-col>
                <v-textarea label="Enter your code here" v-model=code auto-grow></v-textarea>
            </v-col>
        </v-row>

        <v-row>
            <v-col>
                <v-btn @click="calculateComplexity">
                    Calculate cyclomatic complexity
                </v-btn>
            </v-col>

            <v-col>
                Complexity = {{ complexity }}
            </v-col>
        </v-row>

        <v-row>
            <v-col>
                <v-table>
                    <thead>
                        <tr>
                            <th class="text-left">
                                Description
                            </th>
                            <th class="text-left">
                                Ocurrences found
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in patterns" :key="item.description">
                            <td>{{ item.description }}</td>
                            <td>{{ item.count }}</td>
                        </tr>
                    </tbody>
                </v-table>
            </v-col>
        </v-row>
    </v-container>
</template>


<script setup>

import { ref, computed } from 'vue'

const complexity = ref(0)

const code = ref("")

const patterns = ref([
    {
        regx: /\bif\b/gi,
        count: 0,
        description: "IF statements"
    },
    {
        regx: /\bfor\b/gi,
        count: 0,
        description: "FOR statements"
    },
    {
        regx: /\bwhile\b/gi,
        count: 0,
        description: "WHILE statements"
    },
    {
        regx: /\bcase\b/gi,
        count: 0,
        description: "CASE statements"
    }
])

function calculateComplexity() {
    let cmplx = 0

    for (let i = 0; i < patterns.value.length; i++) {
        let pattern = patterns.value[i]
        let matches = pattern.regx.exec(code.value)
        if (matches == null) {
            patterns.value[i].count = 0
        } else {
            patterns.value[i].count = matches.length
        }

        cmplx += patterns.value[i].count
    }

    complexity.value = cmplx + 1
}

</script>