<script setup>
import { ref } from 'vue'

const reviews = [
  { id: 1, nome: "João Silva", nota: 5, texto: "Melhor degradê da região! O atendimento é nota 10.", foto: "https://i.pravatar.cc/150?u=1" },
  { id: 2, nome: "Pedro Santos", nota: 5, texto: "Ambiente muito massa e a cerveja está sempre gelada.", foto: "https://i.pravatar.cc/150?u=2" },
  { id: 3, nome: "Lucas Moraes", nota: 5, texto: "Profissionais qualificados. Recomendo o corte com a navalha.", foto: "https://i.pravatar.cc/150?u=3" },
  { id: 4, nome: "Marcos Souza", nota: 4, texto: "Virei cliente fiel. Preço justo e qualidade impecável.", foto: "https://i.pravatar.cc/150?u=4" }
];

const secaoAtiva = ref(0)
const scrollY = ref(0)

const handleScroll = (event) => {
  const top = event.target.scrollTop
  scrollY.value = top
  secaoAtiva.value = Math.round(top / window.innerHeight)
}

const carrosselRef = ref(null)
const scrollLeft = () => { carrosselRef.value.scrollBy({ left: -300, behavior: 'smooth' }) }
const scrollRight = () => { carrosselRef.value.scrollBy({ left: 300, behavior: 'smooth' }) }
</script>

<template>
  <div class="fixed right-4 md:right-6 top-1/2 -translate-y-1/2 z-50 hidden sm:flex flex-col gap-5">
    <div 
      v-for="(ponto, index) in 4" :key="index"
      class="w-2 md:w-2.5 h-2 md:h-2.5 rounded-full border border-amber-500 transition-all duration-500"
      :class="secaoAtiva === index ? 'bg-amber-500 scale-150 shadow-[0_0_12px_#f59e0b]' : 'bg-transparent opacity-40'"
    ></div>
  </div>

  <div 
    @scroll="handleScroll"
    class="h-screen w-full overflow-y-auto snap-y snap-mandatory bg-black text-white font-sans scroll-smooth hide-scrollbar"
  >
    
    <section class="h-screen w-full relative flex items-center justify-center snap-start overflow-hidden px-6">
      <img 
        class="absolute inset-0 h-full w-full object-cover opacity-50 will-change-transform transition-transform duration-200 ease-out" 
        src="/teste.jpg" 
        :style="{ transform: `translateY(${scrollY * 0.25}px)` }" 
      >
      <div 
        class="relative z-10 text-center transition-all duration-300 w-full"
        :style="{ transform: `translateY(${scrollY * -0.3}px)`, opacity: 1 - scrollY / 600 }"
      >
        <h1 class="text-5xl sm:text-7xl md:text-8xl lg:text-9xl font-black uppercase tracking-tighter italic leading-none">
          Corte & <span class="text-amber-500 text-outline">Tesoura</span>
        </h1>
        <p class="mt-4 text-zinc-300 text-sm md:text-2xl font-light tracking-[0.3em] uppercase">
          Agendamentos Online
        </p>
        <div class="mt-8 md:mt-12 animate-bounce">
          <span class="text-amber-500 text-2xl md:text-3xl">↓</span>
        </div>
      </div>
    </section>

    <section class="h-screen w-full flex items-center justify-center snap-start bg-zinc-950 p-4 md:p-6 relative z-20 shadow-[0_-50px_100px_rgba(0,0,0,0.9)]">
      <div class="w-full max-w-md bg-zinc-900 border border-zinc-800 p-6 md:p-10 rounded-[2rem] md:rounded-[2.5rem] shadow-2xl">
        <div class="text-center mb-6 md:mb-10">
          <div class="w-14 h-14 md:w-20 md:h-20 bg-amber-500 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl md:text-3xl shadow-[0_0_30px_rgba(245,158,11,0.3)]">
            ✂️
          </div>
          <h2 class="text-3xl md:text-4xl font-black uppercase italic tracking-tight">Reserva</h2>
        </div>

        <div class="space-y-4 md:space-y-5 text-start">
          <div class="group">
            <label class="text-zinc-500 text-[10px] uppercase font-black ml-2 mb-1 block group-focus-within:text-amber-500 tracking-widest">Nome Completo</label>
            <input type="text" placeholder="Ex: Ricardo Silva" class="w-full bg-zinc-800/50 border border-zinc-700 rounded-xl md:rounded-2xl p-4 md:p-5 text-white text-sm outline-none focus:border-amber-500 transition-all shadow-inner">
          </div>
          <div class="group">
            <label class="text-zinc-500 text-[10px] uppercase font-black ml-2 mb-1 block group-focus-within:text-amber-500 tracking-widest">E-mail</label>
            <input type="email" placeholder="seu@email.com" class="w-full bg-zinc-800/50 border border-zinc-700 rounded-xl md:rounded-2xl p-4 md:p-5 text-white text-sm outline-none focus:border-amber-500 transition-all shadow-inner">
          </div>
          <div class="group">
            <label class="text-zinc-500 text-[10px] uppercase font-black ml-2 mb-1 block group-focus-within:text-amber-500 tracking-widest">WhatsApp</label>
            <input type="tel" placeholder="(11) 99999-9999" class="w-full bg-zinc-800/50 border border-zinc-700 rounded-xl md:rounded-2xl p-4 md:p-5 text-white text-sm outline-none focus:border-amber-500 transition-all shadow-inner">
          </div>
        </div>

        <button class="w-full bg-amber-600 hover:bg-amber-500 text-white font-black py-4 md:py-6 rounded-xl md:rounded-2xl mt-8 md:mt-10 transition-all active:scale-[0.98] shadow-xl uppercase tracking-widest text-xs md:text-sm">
          Confirmar Agendamento
        </button>
      </div>
    </section>

    <section class="h-screen w-full flex items-center justify-center snap-start bg-white text-black p-4 md:p-6 overflow-hidden">
      <div class="max-w-6xl w-full">
        <div class="flex flex-col sm:flex-row items-center sm:items-end justify-between mb-8 md:mb-12 gap-4">
          <div class="text-center sm:text-left">
            <h2 class="text-3xl md:text-4xl font-black uppercase italic tracking-tight">O que dizem os</h2>
            <p class="text-amber-600 text-4xl md:text-5xl font-black uppercase italic leading-none">Clientes</p>
          </div>
          <div class="flex gap-2">
            <button @click="scrollLeft" class="w-10 h-10 md:w-12 md:h-12 rounded-full border-2 border-zinc-900 flex items-center justify-center hover:bg-zinc-900 hover:text-white transition-all active:scale-90">←</button>
            <button @click="scrollRight" class="w-10 h-10 md:w-12 md:h-12 rounded-full border-2 border-zinc-900 flex items-center justify-center hover:bg-zinc-900 hover:text-white transition-all active:scale-90">→</button>
          </div>
        </div>

        <div ref="carrosselRef" class="flex gap-4 md:gap-6 overflow-x-auto snap-x scrollbar-hide pb-6">
          <div v-for="item in reviews" :key="item.id" class="min-w-[90%] sm:min-w-[45%] lg:min-w-[calc(33.333%-1.5rem)] snap-center bg-zinc-50 p-6 md:p-8 rounded-3xl shadow-xl shrink-0 flex flex-col justify-between border border-zinc-200">
            <div class="text-left">
              <div class="text-amber-500 mb-3 text-lg md:text-xl">★★★★★</div>
              <p class="text-zinc-600 italic text-base md:text-lg leading-relaxed">"{{ item.texto }}"</p>
            </div>
            <div class="mt-6 flex items-center gap-4 border-t border-zinc-100 pt-4">
              <img :src="item.foto" class="w-10 h-10 md:w-12 md:h-12 rounded-full border border-zinc-200 grayscale">
              <span class="font-bold uppercase tracking-tighter text-zinc-900 text-sm md:text-base">{{ item.nome }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <footer class="min-h-screen w-full flex flex-col snap-start bg-black border-t border-zinc-900 relative">
      <div class="flex-1 flex flex-col items-center justify-center px-6 py-12">
        <div class="text-center w-full max-w-4xl">
          <h2 class="text-amber-500 text-4xl sm:text-6xl md:text-7xl font-black uppercase italic mb-8 md:mb-12 tracking-tighter">
            Corte & Tesoura
          </h2>
          
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-10 md:gap-20 text-zinc-400 uppercase text-[10px] md:text-sm tracking-[0.2em] font-bold">
            <div class="space-y-3">
              <p class="text-white border-b border-amber-500/30 pb-2 inline-block">Localização</p>
              <p class="leading-relaxed">Rua das Barbearias, 123<br>São Paulo - SP</p>
            </div>
            <div class="space-y-3">
              <p class="text-white border-b border-amber-500/30 pb-2 inline-block">Horários</p>
              <p class="leading-relaxed">Seg - Sex: 09h às 20h<br>Sáb: 08h às 18h</p>
            </div>
          </div>

          <div class="mt-12 md:mt-20 flex flex-wrap items-center justify-center gap-6 md:gap-10">
             <a href="#" class="text-zinc-600 hover:text-amber-500 transition-colors text-[9px] md:text-[10px] tracking-[0.3em] font-black underline md:no-underline">INSTAGRAM</a>
             <a href="#" class="text-zinc-600 hover:text-amber-500 transition-colors text-[9px] md:text-[10px] tracking-[0.3em] font-black underline md:no-underline">WHATSAPP</a>
          </div>
        </div>
      </div>

      <div class="py-6 md:py-10 bg-zinc-950 border-t border-zinc-900/50 w-full px-6 md:px-8 flex flex-col md:flex-row items-center justify-between gap-4 md:gap-6 text-center md:text-left">
        <div class="flex items-center gap-3">
          <span class="relative flex h-2 w-2">
            <span class="animate-ping absolute h-full w-full rounded-full bg-green-400 opacity-75"></span>
            <span class="relative inline-flex rounded-full h-2 w-2 bg-green-500"></span>
          </span>
          <span class="text-zinc-600 text-[8px] md:text-[9px] uppercase font-black tracking-[0.3em]">Disponível Agora</span>
        </div>
        <p class="text-zinc-800 text-[8px] md:text-[9px] uppercase font-black tracking-[0.3em]">© 2026 - Todos os direitos reservados</p>
      </div>
    </footer>

  </div>
</template>

<style>
/* Estilos globais inalterados, apenas otimização de renderização */
body, html { margin: 0; padding: 0; overflow: hidden; height: 100%; -webkit-tap-highlight-color: transparent; }
.hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
.hide-scrollbar::-webkit-scrollbar { display: none; }
.snap-y { scroll-snap-type: y mandatory; }
.snap-start { scroll-snap-align: start; scroll-snap-stop: always; }
.text-outline { -webkit-text-stroke: 1px #f59e0b; color: transparent; }
.scrollbar-hide::-webkit-scrollbar { display: none; }
.scrollbar-hide { -ms-overflow-style: none; scrollbar-width: none; }
* { -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; box-sizing: border-box; }

/* Ajuste fino para telas muito pequenas (iPhone SE, etc) */
@media (max-height: 670px) {
  .snap-start { scroll-snap-align: none; } /* Desativa snap em telas muito baixas para evitar corte de conteúdo */
  section { height: auto !important; min-height: 100vh; }
}
</style>