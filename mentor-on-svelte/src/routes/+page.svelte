<script lang="ts">
  import { onMount } from 'svelte';
  let mentors = [];
  let search = '';
  let filter = 'Todos';
  let dropdownOpen = false;

  const mentorsData = [
    { name: 'Carla Pereira', skill: 'Inglês', avatar: 'https://randomuser.me/api/portraits/women/44.jpg', rating: 5, reviews: 43, price: 30.0 },
    { name: 'João Silva', skill: 'Programação', avatar: 'https://randomuser.me/api/portraits/men/32.jpg', rating: 5, reviews: 200, price: 40.0 },
    { name: 'Lucas Souza', skill: 'Matemática', avatar: 'https://randomuser.me/api/portraits/men/65.jpg', rating: 5, reviews: 188, price: 35.0 },
    { name: 'Ana Martins', skill: 'Redação', avatar: 'https://randomuser.me/api/portraits/women/68.jpg', rating: 4, reviews: 120, price: 28.0 },
    { name: 'Pedro Lima', skill: 'Física', avatar: 'https://randomuser.me/api/portraits/men/45.jpg', rating: 5, reviews: 99, price: 38.0 },
    { name: 'Marina Costa', skill: 'Química', avatar: 'https://randomuser.me/api/portraits/women/12.jpg', rating: 4, reviews: 87, price: 32.0 },
    { name: 'Rafael Torres', skill: 'História', avatar: 'https://randomuser.me/api/portraits/men/23.jpg', rating: 5, reviews: 76, price: 27.0 },
    { name: 'Beatriz Rocha', skill: 'Biologia', avatar: 'https://randomuser.me/api/portraits/women/51.jpg', rating: 5, reviews: 110, price: 33.0 },
  ];

  $: filteredMentors = mentorsData.filter((mentor) => {
    const matchesSearch = mentor.name.toLowerCase().includes(search.toLowerCase()) || mentor.skill.toLowerCase().includes(search.toLowerCase());
    const matchesFilter = filter === 'Todos' || mentor.skill === filter;
    return matchesSearch && matchesFilter;
  });

  let carouselRef: HTMLElement;
  let benefitsRefs = [];
  let ctaRef: HTMLElement;

  onMount(() => {
    const handleScroll = () => {
      benefitsRefs.forEach((item, idx) => {
        if (!item) return;
        const rect = item.getBoundingClientRect();
        if (rect.top < window.innerHeight - 100 && !item.classList.contains('in-view')) {
          setTimeout(() => item.classList.add('in-view'), idx * 200);
        }
      });
      if (ctaRef) {
        const rect = ctaRef.getBoundingClientRect();
        if (rect.top < window.innerHeight - 100) {
          ctaRef.classList.add('in-view');
        }
      }
    };

    window.addEventListener('scroll', handleScroll);
    handleScroll();
    return () => window.removeEventListener('scroll', handleScroll);
  });
</script>

<div class="MentorOn">
  <main>
    <div class="main-title">
      Conectando pessoas que querem<br />
      aprender com mentores que ensinam
    </div>
    <div class="subtitle">
      Encontre mentores experientes para aprender de forma<br />
      rápida e personalizada
    </div>

    <form class="search-bar" on:submit|preventDefault>
      <div class="search-input">
        <svg width="24" height="24" fill="none" stroke="#888" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" viewBox="0 0 24 24"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
        <input type="text" placeholder="Buscar por habilidade" bind:value={search} />
      </div>
      <div class="custom-dropdown">
        <button
          type="button"
          class={"search-select custom-dropdown-btn" + (dropdownOpen ? ' open' : '')}
          on:click={() => dropdownOpen = !dropdownOpen}
          on:blur={() => setTimeout(() => dropdownOpen = false, 150)}
        >
          {filter}
          <span class="dropdown-arrow">⌵</span>
        </button>
        {#if dropdownOpen}
          <ul class="custom-dropdown-list">
            {#each [
              'Todos', 'Inglês', 'Espanhol', 'Francês', 'Alemão', 'Mandarim', 'Japonês',
              'Programação', 'Desenvolvimento Web', 'JavaScript', 'Python', 'Java',
              'C++', 'React', 'Node.js', 'Banco de Dados',
              'Matemática', 'Cálculo', 'Álgebra Linear', 'Estatística', 'Física',
              'Química', 'Biologia', 'Redação', 'Português', 'Gramática', 'Literatura',
              'História', 'Geografia', 'Filosofia', 'Sociologia', 'Marketing Digital',
              'Design UX/UI', 'Edição de Vídeo', 'Excel Avançado', 'Investimentos',
              'Finanças Pessoais', 'Produtividade', 'Carreira e Entrevistas'
            ] as opt}
              <li
                class={filter === opt ? 'selected' : ''}
                on:mousedown={() => { filter = opt; dropdownOpen = false; }}
              >
                {opt}
              </li>
            {/each}
          </ul>
        {/if}
      </div>

    </form>

    <div class="section-title">Mentores em destaque</div>
    <div class="mentors-carousel-outer">
      <div class="mentors-carousel-wrapper-centered">
        {#if filteredMentors.length === 0}
          <div style="color:#888; font-size:1.1rem">Nenhum mentor encontrado.</div>
        {:else}
          <div class="mentors-carousel" bind:this={carouselRef}>
            {#each filteredMentors as mentor, idx}
              <div class="mentor-card">
                 <img class="mentor-avatar" src={mentor.avatar} alt={mentor.name} />
                 <div class="mentor-name">{mentor.name}</div>
                 <div class="mentor-skill">{mentor.skill}</div>
                 <div class="mentor-rating">
                   {#each Array(mentor.rating) as _, i}
                     <span class="star">★</span>
                   {/each}
                   <span class="reviews">({mentor.reviews})</span>
                 </div>
                 <div class="mentor-price">R$ {mentor.price.toFixed(2).replace('.', ',')}</div>
                 <button class="btn-profile">Ver perfil</button>
               </div>
             {/each}
           </div>
        {/if}
      </div>
    </div>

    <!-- Benefícios -->
    <section class="benefits-section">
      <h2 class="benefits-title">Por que usar o MentorOn?</h2>
      <div class="benefits-list">
        {#each [
          { icon: '🎯', title: 'Aprendizado Personalizado', text: 'Mentoria sob medida para suas necessidades e ritmo de evolução.' },
          { icon: '💡', title: 'Mentores Experientes', text: 'Profissionais qualificados e avaliados por centenas de alunos.' },
          { icon: '🔒', title: 'Ambiente Seguro', text: 'Plataforma confiável, pagamentos protegidos e suporte dedicado.' }
        ] as benefit, idx}
          <div class="benefit-item" bind:this={benefitsRefs[idx]}>
            <span class="benefit-icon">{benefit.icon}</span>
            <div>
              <h3>{benefit.title}</h3>
              <p>{benefit.text}</p>
            </div>
          </div>
        {/each}
      </div>
    </section>

    <!-- CTA -->
    <section class="cta-section" bind:this={ctaRef}>
      <h2>Pronto para acelerar seu aprendizado?</h2>
      <p>Encontre o mentor ideal e comece hoje mesmo sua jornada de evolução!</p>
      <button class="btn btn-primary">Quero encontrar um mentor</button>
    </section>

  </main>

</div>
