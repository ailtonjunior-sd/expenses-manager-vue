<template>
    <main>
        <div class="entries-box mx-96">
            <div class="entries-date flex direction-col items-center w-full py-2">
                <a href="#" class="p-2 rounded-full text-gray-400 hover:text-gray-600 hover:bg-gray-300 transition-all ease-in-out duration-150"><ChevronLIcon class="fill-current" /></a>
                <p class="grow text-center p-2 rounded w-full cursor-pointer text-gray-400 font-medium hover:bg-gray-300 hover:text-gray-600">{{ date_selected }}</p>
                <a href="#" class="p-2 rounded-full text-gray-400 hover:text-gray-600 hover:bg-gray-300 transition-all ease-in-out duration-150"><ChevronRIcon class="fill-current" /></a>
            </div>
            <div class="entries-list bg-white filter drop-shadow rounded p-5">
                <div class="btn-actions grid gap-2 grid-cols-2 mb-5">
                    <button class="p-2 bg-red-500 text-white font-medium rounded drop-shadow filter hover:bg-red-700" @click="setIsOpen(true)">+ Nova Despesa</button>
                    <button class="p-2 bg-green-500 text-white font-medium rounded drop-shadow filter hover:bg-green-700">+ Nova Receita</button>
                </div>
                <ul class="list">
                    <li v-for="entrie in entries" :key="entrie.id" class="inline">
                        <a href="#" class="flex direction-col h-18 py-1 items-center rounded hover:bg-gray-100 transition-all ease-in-out duration-150 mb-2">
                            <div class="icon-entrie flex-none px-2 text-gray-400 font-base">{{ entrie.date }}</div>
                            <div class="icon-entrie flex-none px-5">
                                <ArrowCircleDIcon v-if="entrie.expense" class="fill-current h-7 w-7 text-red-500" />
                                <ArrowCircleUIcon v-else class="fill-current h-7 w-7 text-green-500" />
                            </div>
                            <div class="entrie_description grow">
                                <p class="font-medium">{{ formatCurrency(entrie.value) }}</p>
                                <span class="description block text-sm text-gray-400 font-medium">{{ entrie.description }}</span>
                            </div>
                            <a class="actions flex-none mr-2 p-2 text-gray-400 rounded-full cursor-pointer hover:bg-gray-300 hover:text-gray-600">
                                <ThumbsUpIcon v-if="entrie.paid" class="fill-current" />
                                <ThumbsDownIcon v-else class="fill-current" />
                            </a>
                        </a>
                    </li>
                </ul>
            </div>
            <div class="entries-list bg-white filter drop-shadow rounded p-5 mt-2">
                <div class="footer-entries">
                    <div class="footer-entries-list text-gray-400 font-medium">
                    <div class="flex flex-row">
                        <p class="grow">Total a pagar:</p>
                        <p>R$ 1.000,00</p>
                    </div>
                    <div class="flex flex-row">
                        <p class="grow">Total a receber:</p>
                        <p>R$ 1.000,00</p>
                    </div>
                    <div class="flex flex-row">
                        <p class="grow">Restante:</p>
                        <p>R$ 1.000,00
                        </p>
                    </div>
                    </div>
                </div>
            </div>
        </div>
        
        <TransitionRoot appear :show="isOpen" as="template">
            <Dialog as="div">
                <div class="fixed inset-0 z-10 overflow-y-auto">
                    <div class="min-h-screen px-4 text-center">
                        <TransitionChild
                            as="template"
                            enter="duration-300 ease-out"
                            enter-from="opacity-0"
                            enter-to="opacity-100"
                            leave="duration-200 ease-in"
                            leave-from="opacity-100"
                            leave-to="opacity-0"
                        >
                            <DialogOverlay class="bg-black opacity-60 fixed inset-0" />
                        </TransitionChild>

                        <span class="inline-block h-screen align-middle" aria-hidden="true">
                            &#8203;
                        </span>

                        <TransitionChild
                            as="template"
                            enter="duration-300 ease-out"
                            enter-from="opacity-0 scale-95"
                            enter-to="opacity-100 scale-100"
                            leave="duration-200 ease-in"
                            leave-from="opacity-100 scale-100"
                            leave-to="opacity-0 scale-95"
                        >
                            <div class="inline-block w-full max-w-md p-6 my-8 overflow-hidden text-left align-middle transition-all transform bg-white shadow-xl rounded-2xl">
                                <DialogTitle as="h3" class="text-lg font-medium leading-6 text-gray-900">Payment successful</DialogTitle>
                                <div class="mt-2">
                                    <p class="text-sm text-gray-500">
                                        Your payment has been successfully submitted. We’ve sent you
                                        an email with all of the details of your order.
                                    </p>
                                </div>

                                <div class="mt-4">
                                    <button type="button"
                                        class="inline-flex justify-center px-4 py-2 text-sm font-medium text-blue-900 bg-blue-100 border border-transparent rounded-md hover:bg-blue-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-blue-500"
                                        @click="setIsOpen(false)"
                                    >
                                        Got it, thanks!
                                    </button>
                                </div>
                            </div>
                        </TransitionChild>
                    </div>
                </div>
            </Dialog>
        </TransitionRoot>
    </main>
</template>

<script>
import ArrowCircleUIcon from '@/components/icons/ArrowCircleUIcon.vue'
import ArrowCircleDIcon from '@/components/icons/ArrowCircleDIcon.vue'
import ThumbsDownIcon from '@/components/icons/ThumbsDownIcon.vue'
import ThumbsUpIcon from '@/components/icons/ThumbsUpIcon.vue'
import ChevronLIcon from '@/components/icons/ChevronLIcon.vue'
import ChevronRIcon from '@/components/icons/ChevronRIcon.vue'
import { ref } from 'vue'
import { TransitionRoot, TransitionChild, Dialog, DialogOverlay, DialogTitle } from '@headlessui/vue'

export default {
    components: {
        ArrowCircleUIcon,
        ArrowCircleDIcon,
        ThumbsDownIcon,
        ThumbsUpIcon,
        ChevronLIcon,
        ChevronRIcon,
        TransitionRoot,
        TransitionChild,
        Dialog,
        DialogOverlay,
        DialogTitle
    },
    setup() {
        const isOpen = ref(false)
        function setIsOpen(value) {
            isOpen.value = value
        }
        
        return { 
            isOpen,setIsOpen
        }
    },
    data() {
        return {
            entries: [
                {id: 1, description: 'CPFL', value: 234.50, category: 'shopping', date: '09/02', paid: false, expense: true},
                {id: 2, description: 'Condomínio', value: 254.30, category: 'shopping', date: '10/02', paid: false, expense: false}
            ],
            date_selected: 'Fevereiro - 2022'
        }
    },
    methods: {
        formatCurrency(value) {
            return value.toLocaleString('pt-br', { style: 'currency', currency: 'BRL' })
        }
    },
    computed: {
    }
}
</script>
