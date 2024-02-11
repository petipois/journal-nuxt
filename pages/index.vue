<script setup>
import { ref } from 'vue';
const supabase = useSupabaseClient();
const entries = ref([])
const getEntries = async () => {
    try {

        let { data, error } = await supabase
            .from('journal')
            .select('*')

        if (data) {
            entries.value = data;
        }
        console.log(entries);
        if (error)
            throw error;
    } catch (error) {
        console.log(error)
    }
}
const deleteEntry = async (id) => {

    try {
        const { error } = await supabase
            .from('journal')
            .delete()
            .eq('entry_id', id);
        if (error)
            throw error;
        getEntries();
    } catch (error) {
        console.log(error);
    }
}
onBeforeMount(async () => {
    try {

        getEntries();
    } catch (error) {

    }
})
</script>
<template>
    <section class="mx-auto h-screen py-12">
        <div class="py-8 px-4 mx-auto max-w-2xl lg:py-16" v-if="entries.length > 0">
            <h1 class="text-5xl text-white font-bold tracking-tight mb-4">My Digital Journal</h1>
            <ol class="relative border-s border-gray-200 dark:border-gray-700">
                <li class="mb-10 ms-4" v-for="entry in entries">
                    <div
                        class="absolute w-3 h-3 bg-gray-200 rounded-full mt-1.5 -start-1.5 border border-white dark:border-gray-900 dark:bg-gray-700">
                    </div>
                    <datetime class="mb-1 text-sm font-normal leading-none text-gray-400 dark:text-gray-500">{{ new
                        Date(entry.entry_date).toDateString() }}</datetime>
                    <h3 class="text-lg font-semibold text-gray-900 dark:text-white">{{ entry.title }}</h3>
                    <p class="mb-4 text-base font-normal text-gray-500 dark:text-gray-400">{{ entry.content }}</p>
                    <button @click="deleteEntry(entry.entry_id)"
                        class="inline-flex items-center px-4 py-2 text-sm font-medium text-white bg-red-900 border border-gray-200 rounded-lg hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-4 focus:outline-none">Delete
                        entry</button>
                </li>

            </ol>

        </div>
        <div class="py-8 px-4 mx-auto max-w-2xl lg:py-16" v-else>


            <div
                class="flex items-center justify-center h-56 border border-gray-200 rounded-lg bg-gray-800">
                <div
                    class="px-3 py-1 text-xs font-medium leading-none text-center text-blue-800 bg-gray-200 rounded-full animate-pulse">
                    loading entries...</div>
            </div>
        </div>

    </section>
</template>