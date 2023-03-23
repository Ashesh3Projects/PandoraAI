<script setup>
// eslint-disable-next-line import/no-extraneous-dependencies
import 'fork-corner/dist/fork-corner.css';
import { storeToRefs } from 'pinia';
import initForkCorner from '~/fork-corner';
import AppSidebar from '~/components/AppSidebar.vue';
import { useAppStore } from '~/stores/app';
import { useConversationsStore } from '~/stores/conversations';

const appStore = useAppStore();
const conversationsStore = useConversationsStore();

const {
    isMenuOpen,
    isMenuOpening,
} = storeToRefs(appStore);

const {
    conversationTitle,
} = storeToRefs(conversationsStore);

useHead({
    title: `${conversationTitle.value} - PandoraAI`,
    meta: [
        {
            name: 'viewport',
            content: 'width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no',
        },
    ],
});

onMounted(() => {
    initForkCorner();
});
</script>

<template>
    <link href='https://fonts.googleapis.com/css?family=Roboto' rel='stylesheet'>
    <div class="flex flex-row min-h-screen text-white">
        <AppSidebar />
        <Transition name="fade">
            <div v-if="isMenuOpen" @click="isMenuOpen = false" class="fixed inset-0 bg-black/30 z-10 lg:hidden"
                aria-hidden="true" />
        </Transition>
        <div
            class="flex flex-col flex-1 w-full"
            :class="{
                'lg:ml-[20rem] transition-all ease-in-out': isMenuOpening,
                'lg:ml-0': !isMenuOpening,
            }"
        >
            <header class="px-3 py-6 text-center bg-[#2a2828] shadow relative">
                <!-- Menu icon -->
                <button @click="isMenuOpen = true" class="
                            absolute top-0 left-0 p-3 ml-3 mt-3 text-white/70 hover:text-white/90 z-10
                            focus:outline-none
                            transition duration-300 ease-in-out
                            lg:hidden
                        ">
                    <Icon name="bx:bx-menu" class="w-8 h-8" />
                </button>
                <!-- Show/Hide sidebar button (lg) -->
                <button @click="isMenuOpen = !isMenuOpen" class="
                            absolute top-0 left-0 p-3 ml-3 mt-3 text-white/70 hover:text-white/90 z-10
                            focus:outline-none
                            transition duration-300 ease-in-out
                            hidden lg:block
                        ">
                    <Icon :name="isMenuOpen ? 'bx:bx-chevron-left' : 'bx:bx-chevron-right'" class="w-8 h-8" />
                </button>
                <!-- Title -->
                <h1 class="text-2xl font-bold text-white/80 drop-shadow-md">Pandora<span class="font-light">AI</span></h1>
            </header>
            <main class="flex flex-grow py-2">
                <Chat />
            </main>
            <footer class="px-3 pb-6 text-center">
                <span class="text-xs font-light text-slate-400">
                </span>
            </footer>
        </div>
    </div>
</template>

<style>
:root {
    --background: #121212
}

body {
    margin: 0;
    color: #fff;
    font-family: "Roboto", "Helvetica", "Arial", sans-serif;
    font-weight: 400;
    font-size: 1rem;
    line-height: 1.5;
    letter-spacing: 0.00938em;
    background-color: #121212;
}

footer a {
    @apply opacity-75 hover:opacity-100 transition duration-300 ease-in-out;
    background: linear-gradient(to right, #37feff 0%, #bd7bff 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.15s ease-in-out;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}

::-webkit-scrollbar {
    width: 9px;
}

::-webkit-scrollbar-track {
    background: transparent;
}

::-webkit-scrollbar-thumb {
    background-color: rgba(155, 155, 155, 0.5);
    border-radius: 20px;
    border: transparent;
}
</style>
