<template>
    <div class="button-sumario-position">
        <button class="button-sumario animate-nav-button" @click="toggleSumario" aria-label="Abrir sumário">
            <div class="hamburger" :class="{ 'active': isSumarioOpen }">
                <span></span>
                <span></span>
                <span></span>
            </div>
        </button>

        <transition name="sumario">
            <div v-if="isSumarioOpen" class="sumario-container">
                <div class="sumario-overlay" @click="toggleSumario"></div>
                <div class="sumario-lista">
                    <h3 class="sumario-titulo">Sumário</h3>
                    <ul>
                        <li v-for="(topico, index) in topicos" :key="index">
                            <a :href="topico.link" @click="navigateTo(topico.link, $event)" class="sumario-link">
                                {{ topico.titulo }}
                            </a>
                        </li>
                    </ul>
                </div>
            </div>
        </transition>
    </div>
</template>

<script>
export default {
    data() {
        return {
            isSumarioOpen: false,
            topicos: [
                { titulo: "Capa", link: 0 },
                { titulo: "História do Vue.js", link: 1 },
                { titulo: "Origem e Criação (2013–2014)", link: 2 },
                { titulo: "Crescimento e Adoção (2015–2016)", link: 3 },
                { titulo: "Vue 2.0 – Consolidação (2016)", link: 4 },
                { titulo: "Popularidade Mundial (2017–2019)", link: 5 },
                { titulo: "Vue 3.0 – Evolução Moderna (2020)", link: 6 },
                { titulo: "Vue.js Atualmente (2021–2025)", link: 7 },
                { titulo: "Vantagens do Vue.js", link: 8 },
                { titulo: "Desvantagens do Vue.js", link: 9 },
                { titulo: "Áreas onde o Vue.js é bem utilizado", link: 10 },
                { titulo: "Empresas que usam Vue.js", link: 11 },
                { titulo: "Fluxograma do Projeto", link: 12 }
            ]
        };
    },
    methods: {
        toggleSumario() {
            this.isSumarioOpen = !this.isSumarioOpen;
        },
        navigateTo(slideIndex, event) {
            event.preventDefault(); // Previne o comportamento padrão do link
            this.isSumarioOpen = false; // Fecha o sumário
            this.$emit("mudar-slide", slideIndex); // Emite um evento para o App.vue
        },
    }
};
</script>

<style>
.button-sumario {
    position: fixed;
    top: 20px;
    left: 20px;
    z-index: 1000;
    width: 60px;
    height: 60px;
    background: #2a2a2a;
    border: none;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
    cursor: pointer;
    transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
    padding: 0;
}

.button-sumario:focus {
    outline: 2px solid #42b883;
    outline-offset: 2px;
}

.button-sumario:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: 0 6px 14px rgba(0, 0, 0, 0.5);
    background: #353535;
}

/* Estilos para o ícone de hamburger */
.hamburger {
    width: 24px;
    height: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.hamburger span {
    display: block;
    width: 100%;
    height: 3px;
    background: white;
    border-radius: 2px;
    transition: all 0.3s ease;
}

.hamburger.active span:nth-child(1) {
    transform: translateY(8px) rotate(45deg);
}

.hamburger.active span:nth-child(2) {
    opacity: 0;
}

.hamburger.active span:nth-child(3) {
    transform: translateY(-8px) rotate(-45deg);
}

/* Estilos para o sumário */
.sumario-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 999;
}

.sumario-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
}

.sumario-lista {
    position: fixed;
    top: 0;
    left: 0;
    width: 360px;
    min-height: 100%;
    background: #2a2a2a;
    padding: 80px 20px 40px;
    box-shadow: 2px 0 10px rgba(0, 0, 0, 0.2);
    max-height: 100vh;         /* Garante que não ultrapasse a tela */
    overflow-y: auto;          /* Adiciona o scroll vertical */
    box-sizing: border-box; /* garante que o padding não corte o conteúdo */
}

.sumario-lista::-webkit-scrollbar {
    width: 8px;
    background: #232323;
    border-radius: 4px;
}

.sumario-lista::-webkit-scrollbar-thumb {
    background: #42b883;
    border-radius: 4px;
}

.sumario-lista::-webkit-scrollbar-thumb:hover {
    background: #368f6e;
}

.sumario-titulo {
    color: #42b883;
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 1px solid #444;
}

.sumario-lista ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

.sumario-lista li {
    margin-bottom: 12px;
}

.sumario-link {
    color: white;
    text-decoration: none;
    font-size: 16px;
    transition: color 0.2s;
    display: block;
    padding: 8px 12px;
    border-radius: 4px;
}

.sumario-link:hover {
    color: #42b883;
    background: rgba(255, 255, 255, 0.1);
}

/* Transições */
.sumario-enter-active, .sumario-leave-active {
    transition: opacity 0.3s ease;
}

.sumario-enter-active .sumario-lista,
.sumario-leave-active .sumario-lista {
    transition: transform 0.3s ease;
}

.sumario-enter-from,
.sumario-leave-to {
    opacity: 0;
}

.sumario-enter-from .sumario-lista,
.sumario-leave-to .sumario-lista {
    transform: translateX(-100%);
}

.sumario-enter-to .sumario-lista,
.sumario-leave-from .sumario-lista {
    transform: translateX(0);
}
</style>