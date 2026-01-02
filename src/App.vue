<script setup>
import { ref } from 'vue'

// --- ESTADO DO FORMULÁRIO ---
const nome = ref('')
const email = ref('')
const whatsapp = ref('')
const carregando = ref(false)

// --- FUNÇÃO DE RESERVA (COM VALIDAÇÃO DE @ E ID NUMÉRICO) ---
const realizarReserva = () => {
  if (!nome.value || !whatsapp.value || !email.value) {
    alert('Por favor, preencha todos os campos.')
    return
  }

  // VALIDAÇÃO DO @
  if (!email.value.includes('@')) {
    alert('E-mail inválido! O campo deve conter "@".')
    return
  }

  carregando.value = true

  // ID 100% numérico para não dar erro no link
  const idReserva = Math.floor(10000000 + Math.random() * 90000000)

  const mensagemBase = `Olá quero realizar uma reserva. Minha reserva é: *${idReserva}*\n\n` +
                       `*Dados do Cliente:*\n` +
                       `Nome: ${nome.value}\n` +
                       `E-mail: ${email.value}\n` +
                       `WhatsApp: ${whatsapp.value}`;

  const meuNumero = "5511949990317" 
  const linkWhats = `https://api.whatsapp.com/send?phone=${meuNumero}&text=${encodeURIComponent(mensagemBase)}`

  setTimeout(() => {
    window.location.href = linkWhats
    carregando.value = false
  }, 800)
}

// Dados dos Reviews
const reviews = [
  { id: 1, nome: "João Silva", texto: "Melhor degradê da região! O atendimento é nota 10.", foto: "https://i.pravatar.cc/150?u=1" },
  { id: 2, nome: "Pedro Santos", texto: "Ambiente muito massa e a cerveja está sempre gelada.", foto: "https://i.pravatar.cc/150?u=2" },
  { id: 3, nome: "Lucas Moraes", texto: "Profissionais qualificados. Recomendo o corte com a navalha.", foto: "https://i.pravatar.cc/150?u=3" },
  { id: 4, nome: "Marcos Souza", texto: "Virei cliente fiel. Preço justo e qualidade impecável.", foto: "https://i.pravatar.cc/150?u=4" }
];

// --- LÓGICA DE NAVEGAÇÃO ---
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
    <div v-for="(ponto, index) in 4" :key="index"
      class="w-2.5 h-2.5 rounded-full border border-amber-500 transition-all duration-500"
      :class="secaoAtiva === index ? 'bg-amber-500 scale-150 shadow-[0_0_12px_#f59e0b]' : 'bg-transparent opacity-40'"
    ></div>
  </div>

  <div @scroll="handleScroll" class="h-screen w-full overflow-y-auto snap-y snap-mandatory bg-black text-white font-sans scroll-smooth hide-scrollbar">
    
    <section class="section-height w-full relative flex items-center justify-center snap-start overflow-hidden px-6">
      <img class="absolute inset-0 h-full w-full object-cover opacity-50 transition-transform duration-200" src="/teste.jpg" :style="{ transform: `translateY(${scrollY * 0.25}px)` }">
      <div class="relative z-10 text-center" :style="{ transform: `translateY(${scrollY * -0.3}px)`, opacity: 1 - scrollY / 600 }">
        <h1 class="text-5xl sm:text-7xl md:text-8xl lg:text-9xl font-black uppercase tracking-tighter italic leading-tight">
          Corte & <span class="bg-gradient-to-b from-amber-400 to-amber-600 bg-clip-text text-transparent">Tesoura</span>
        </h1>
        <p class="mt-4 text-zinc-300 text-sm md:text-2xl font-light tracking-[0.3em] uppercase">Agendamentos Online</p>
        <div class="mt-12 animate-bounce"><span class="text-amber-500 text-3xl">↓</span></div>
      </div>
    </section>

    <section class="section-height w-full flex items-center justify-center snap-start bg-zinc-950 p-4 relative z-20 shadow-[0_-50px_100px_rgba(0,0,0,0.9)]">
      <div class="w-full max-w-md bg-zinc-900 border border-zinc-800 p-6 md:p-10 rounded-[2.5rem] shadow-2xl">
        <div class="text-center mb-8">
          <div class="w-16 h-16 bg-amber-500 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl shadow-lg">✂️</div>
          <h2 class="text-3xl font-black uppercase italic text-white">Reserva</h2>
        </div>
        <div class="space-y-4">
          <div class="group text-start">
            <label class="text-zinc-500 text-[10px] uppercase font-black ml-2 mb-1 block group-focus-within:text-amber-500 tracking-widest transition-colors">Nome Completo</label>
            <input v-model="nome" type="text" placeholder="Ex: Ricardo Silva" class="w-full bg-zinc-800/50 border border-zinc-700 rounded-2xl p-4 text-white outline-none focus:border-amber-500 transition-all">
          </div>
          <div class="group text-start">
            <label class="text-zinc-500 text-[10px] uppercase font-black ml-2 mb-1 block group-focus-within:text-amber-500 tracking-widest transition-colors">E-mail</label>
            <input v-model="email" type="email" placeholder="seu@email.com" class="w-full bg-zinc-800/50 border border-zinc-700 rounded-2xl p-4 text-white outline-none focus:border-amber-500 transition-all">
          </div>
          <div class="group text-start">
            <label class="text-zinc-500 text-[10px] uppercase font-black ml-2 mb-1 block group-focus-within:text-amber-500 tracking-widest transition-colors">WhatsApp</label>
            <input v-model="whatsapp" type="tel" placeholder="(11) 99999-9999" class="w-full bg-zinc-800/50 border border-zinc-700 rounded-2xl p-4 text-white outline-none focus:border-amber-500 transition-all">
          </div>
        </div>
        <button @click="realizarReserva" :disabled="carregando" class="w-full bg-amber-600 hover:bg-amber-500 py-5 rounded-2xl mt-8 transition-all active:scale-[0.98] font-black uppercase tracking-widest text-sm">
          {{ carregando ? 'Gerando Protocolo...' : 'Confirmar Agendamento' }}
        </button>
      </div>
    </section>

    <section class="section-height w-full flex items-center justify-center snap-start bg-white text-black p-4 md:p-6 overflow-hidden">
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
              <img :src="item.foto" class="w-10 h-10 md:w-12 md:h-12 rounded-full grayscale border border-zinc-200">
              <span class="font-bold uppercase tracking-tighter text-zinc-900 text-sm md:text-base">{{ item.nome }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <footer class="section-height w-full flex flex-col snap-start bg-black border-t border-zinc-900 relative">
      <div class="flex-1 flex flex-col items-center justify-center px-6 py-10">
        <div class="text-center w-full max-w-4xl">
          <h2 class="text-amber-500 text-4xl sm:text-6xl md:text-7xl font-black uppercase italic mb-10 tracking-tighter">Corte & Tesoura</h2>
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-8 md:gap-20 text-zinc-400 uppercase text-[10px] md:text-sm tracking-[0.2em] font-bold">
            <div class="space-y-3">
              <p class="text-white border-b border-amber-500/30 pb-2 inline-block">Localização</p>
              <p class="leading-relaxed text-zinc-500">Rua das Barbearias, 123<br>São Paulo - SP</p>
            </div>
            <div class="space-y-3">
              <p class="text-white border-b border-amber-500/30 pb-2 inline-block">Horários</p>
              <p class="leading-relaxed text-zinc-500">Seg - Sex: 09h às 20h<br>Sáb: 08h às 18h</p>
            </div>
          </div>
        </div>
      </div>
      <div class="py-8 pb-14 md:pb-8 bg-zinc-950 border-t border-zinc-900/50 w-full px-6 md:px-10 flex flex-col md:flex-row items-center justify-between gap-4">
        <div class="flex items-center gap-3">
          <span class="relative flex h-2 w-2">
            <span class="animate-ping absolute h-full w-full rounded-full bg-green-400 opacity-75"></span>
            <span class="relative inline-flex rounded-full h-2 w-2 bg-green-500"></span>
          </span>
          <span class="text-zinc-500 text-[8px] md:text-[9px] uppercase font-black tracking-[0.4em]">Status: Disponível Agora</span>
        </div>
        <p class="text-zinc-800 text-[8px] md:text-[9px] uppercase font-black tracking-[0.4em]">© 2026 - Todos os direitos reservados</p>
      </div>
    </footer>

  </div>
</template>

<style>
body, html { margin: 0; padding: 0; overflow: hidden; height: 100%; background: black; -webkit-tap-highlight-color: transparent; }
.section-height { height: 100vh; height: 100dvh; }
.hide-scrollbar::-webkit-scrollbar { display: none; }
.hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
.snap-y { scroll-snap-type: y mandatory; }
.snap-start { scroll-snap-align: start; scroll-snap-stop: always; }
.scrollbar-hide::-webkit-scrollbar { display: none; }
.scrollbar-hide { -ms-overflow-style: none; scrollbar-width: none; }
* { -webkit-font-smoothing: antialiased; box-sizing: border-box; }
</style>