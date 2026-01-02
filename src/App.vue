<script setup>
import { ref } from 'vue'

// --- ESTADO DO FORMULÁRIO ---
const nome = ref('')
const email = ref('')
const whatsapp = ref('')
const carregando = ref(false)

// --- FUNÇÃO DE RESERVA (COM VALIDAÇÃO DE @ E ID NUMÉRICO) ---
const realizarReserva = () => {
  // 1. Validação de campos vazios
  if (!nome.value || !whatsapp.value || !email.value) {
    alert('Por favor, preencha todos os campos para o agendamento.')
    return
  }

  // 2. OBRIGA O USO DO @ NO EMAIL
  if (!email.value.includes('@')) {
    alert('Por favor, insira um e-mail válido contendo "@".')
    return
  }

  carregando.value = true

  // 3. Gera ID numérico único (8 dígitos) - Evita erros de URL
  const idReserva = Math.floor(10000000 + Math.random() * 90000000)

  // 4. Monta a mensagem para o WhatsApp
  // O asterisco aqui serve apenas para deixar o texto em NEGRITO dentro do WhatsApp
  const mensagemBase = `Olá quero realizar uma reserva. Minha reserva é: *${idReserva}*\n\n` +
                       `*Dados do Cliente:*\n` +
                       `Nome: ${nome.value}\n` +
                       `E-mail: ${email.value}\n` +
                       `WhatsApp: ${whatsapp.value}`;

  // 5. Link do WhatsApp (Troque pelo seu número com DDD)
  const meuNumero = "5511949990317" 
  const linkWhats = `https://api.whatsapp.com/send?phone=${meuNumero}&text=${encodeURIComponent(mensagemBase)}`

  // 6. Redirecionamento
  setTimeout(() => {
    window.location.href = linkWhats
    carregando.value = false
  }, 800)
}

// Dados dos Reviews
const reviews = [
  { id: 1, nome: "João Silva", nota: 5, texto: "Melhor degradê da região! O atendimento é nota 10.", foto: "https://i.pravatar.cc/150?u=1" },
  { id: 2, nome: "Pedro Santos", nota: 5, texto: "Ambiente muito massa e a cerveja está sempre gelada.", foto: "https://i.pravatar.cc/150?u=2" },
  { id: 3, nome: "Lucas Moraes", nota: 5, texto: "Profissionais qualificados. Recomendo o corte com a navalha.", foto: "https://i.pravatar.cc/150?u=3" },
  { id: 4, nome: "Marcos Souza", nota: 4, texto: "Virei cliente fiel. Preço justo e qualidade impecável.", foto: "https://i.pravatar.cc/150?u=4" }
];

// --- LÓGICA DE NAVEGAÇÃO E SCROLL ---
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
      class="w-2.5 h-2.5 rounded-full border border-amber-500 transition-all duration-500"
      :class="secaoAtiva === index ? 'bg-amber-500 scale-150 shadow-[0_0_12px_#f59e0b]' : 'bg-transparent opacity-40'"
    ></div>
  </div>

  <div 
    @scroll="handleScroll"
    class="h-screen w-full overflow-y-auto snap-y snap-mandatory bg-black text-white font-sans scroll-smooth hide-scrollbar"
  >
    
    <section class="section-height w-full relative flex items-center justify-center snap-start overflow-hidden px-6">
      <img 
        class="absolute inset-0 h-full w-full object-cover opacity-50 will-change-transform transition-transform duration-200 ease-out" 
        src="/teste.jpg" 
        :style="{ transform: `translateY(${scrollY * 0.25}px)` }" 
      >
      <div 
        class="relative z-10 text-center transition-all duration-300 w-full"
        :style="{ transform: `translateY(${scrollY * -0.3}px)`, opacity: 1 - scrollY / 600 }"
      >
        <h1 class="text-5xl sm:text-7xl md:text-8xl lg:text-9xl font-black uppercase tracking-tighter italic leading-tight">
          Corte & <span class="bg-gradient-to-b from-amber-400 to-amber-600 bg-clip-text text-transparent drop-shadow-[0_0_15px_rgba(245,158,11,0.4)]">Tesoura</span>
        </h1>
        <p class="mt-4 text-zinc-300 text-sm md:text-2xl font-light tracking-[0.3em] uppercase">
          Agendamentos Online
        </p>
        <div class="mt-8 md:mt-12 animate-bounce">
          <span class="text-amber-500 text-2xl md:text-3xl">↓</span>
        </div>
      </div>
    </section>

    <section class="section-height w-full flex items-center justify-center snap-start bg-zinc-950 p-4 md:p-6 relative z-20 shadow-[0_-50px_100px_rgba(0,0,0,0.9)]">
      <div class="w-full max-w-md bg-zinc-900 border border-zinc-800 p-6 md:p-10 rounded-[2rem] md:rounded-[2.5rem] shadow-2xl">
        <div class="text-center mb-6 md:mb-10">
          <div class="w-16 h-16 md:w-20 md:h-20 bg-amber-500 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl md:text-3xl shadow-[0_0_30px_rgba(245,158,11,0.3)]">
            ✂️
          </div>
          <h2 class="text-3xl md:text-4xl font-black uppercase italic tracking-tight text-white">Reserva</h2>
        </div>

        <div class="space-y-4 md:space-y-5 text-start">
          <div class="group">
            <label class="text-zinc-500 text-[10px] uppercase font-black ml-2 mb-1 block group-focus-within:text-amber-500 tracking-widest transition-colors">Nome Completo</label>
            <input v-model="nome" type="text" placeholder="Ex: Ricardo Silva" class="w-full bg-zinc-800/50 border border-zinc-700 rounded-xl md:rounded-2xl p-4 md:p-5 text-white text-sm outline-none focus:border-amber-500 focus:bg-zinc-800 transition-all shadow-inner">
          </div>
          <div class="group">
            <label class="text-zinc-500 text-[10px] uppercase font-black ml-2 mb-1 block group-focus-within:text-amber-500 tracking-widest transition-colors">E-mail</label>
            <input v-model="email" type="email" placeholder="seu@email.com" class="w-full bg-zinc-800/50 border border-zinc-700 rounded-xl md:rounded-2xl p-4 md:p-5 text-white text-sm outline-none focus:border-amber-500 focus:bg-zinc-800 transition-all shadow-inner">
          </div>
          <div class="group">
            <label class="text-zinc-500 text-[10px] uppercase font-black ml-2 mb-1 block group-focus-within:text-amber-500 tracking-widest transition-colors">WhatsApp</label>
            <input v-model="whatsapp" type="tel" placeholder="(11) 99999-9999" class="w-full bg-zinc-800/50 border border-zinc-700 rounded-xl md:rounded-2xl p-4 md:p-5 text-white text-sm outline-none focus:border-amber-500 focus:bg-zinc-800 transition-all shadow-inner">
          </div>
        </div>

        <button 
          @click="realizarReserva"
          :disabled="carregando"
          class="w-full bg-amber-600 hover:bg-amber-500 disabled:bg-zinc-700 text-white font-black py-4 md:py-6 rounded-xl md:rounded-2xl mt-8 md:mt-10 transition-all active:scale-[0.98] shadow-xl shadow-amber-900/20 uppercase tracking-widest text-xs md:text-sm"
        >
          {{ carregando ? 'Gerando Protocolo...' : 'Confirmar Agendamento' }}
        </button>
      </div>
    </section>

    <section class="section-height w-full flex items-center justify-center snap-start bg-white text-black p-4 md:p-6 overflow-hidden">
      </section>

    <footer class="section-height w-full flex flex-col snap-start bg-black border-t border-zinc-900 relative">
      </footer>

  </div>
</template>

<style>
/* Estilos permanecem iguais */
body, html {
  margin: 0; padding: 0; overflow: hidden; height: 100%;
  background-color: black; -webkit-tap-highlight-color: transparent;
}
.section-height { height: 100vh; height: 100dvh; }
.hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
.hide-scrollbar::-webkit-scrollbar { display: none; }
.snap-y { scroll-snap-type: y mandatory; }
.snap-start { scroll-snap-align: start; scroll-snap-stop: always; }
.scrollbar-hide::-webkit-scrollbar { display: none; }
.scrollbar-hide { -ms-overflow-style: none; scrollbar-width: none; }
* { -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; box-sizing: border-box; }
</style>