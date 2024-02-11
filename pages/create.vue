<script setup>
import { ref } from 'vue';
const supabase = useSupabaseClient();
const entry = ref({
    title: "",
    content: ""
})
const router = useRouter();
const addEntry = async () => {
    try {

        const { data, error } = await supabase
            .from('journal')
            .insert([
                { title: entry.value.title, content: entry.value.content },
            ])
            .select()
        if (error)
            throw error;
        else {
            entry.value.title = "";
            entry.value.content = "";
            router.replace("/");
        }
    } catch (error) {
        console.log(error);
    }
}

</script>
<template>
    <section class="mx-auto h-screen py-12">
        <div class="py-8 px-4 mx-auto max-w-2xl lg:py-16">
            <h2 class="mb-4 text-4xl font-bold text-white">Add a new entry</h2>
            <form @submit.prevent="addEntry">
                <div class="grid gap-4 sm:grid-cols-2 sm:gap-6">
                    <div class="sm:col-span-2">
                        <label for="name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Entry
                            Title</label>
                        <input type="text" v-model="entry.title"
                            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500"
                            placeholder="Type entry title" required="">

                    </div>
                    <div class="sm:col-span-2">
                        <label for="content"
                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Description</label>
                        <textarea rows="8" v-model="entry.content"
                            class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-primary-500 focus:border-primary-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-primary-500 dark:focus:border-primary-500"
                            placeholder="Your description here"></textarea>
                    </div>
                </div>
                <button type="submit"
                    class="bg-gray-500 m-2 inline-flex items-center px-5 py-2.5 mt-4 sm:mt-6 text-sm font-medium text-center text-white bg-primary-700 rounded-lg focus:ring-4 focus:ring-primary-200 dark:focus:ring-primary-900 hover:bg-primary-800">
                    Add entry
                </button>
                <button type="reset"
                    class="bg-red-900 inline-flex items-center px-5 py-2.5 mt-4 sm:mt-6 text-sm font-medium text-center text-white bg-primary-700 rounded-lg focus:ring-4 focus:ring-primary-200 dark:focus:ring-primary-900 hover:bg-primary-800">
                    Clear form
                </button>
            </form>
        </div>
    </section>
</template>