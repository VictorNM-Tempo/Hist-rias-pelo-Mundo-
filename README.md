<!DOCTYPE html><html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Histórias para Compartilhar</title>
    <style>
        body {
            margin: 0;
            font-family: 'Georgia', serif;
            background-color: #f4f1ee;
            color: #3e3e3e;
        }
        header {
            background-image: url('https://images.unsplash.com/photo-1516972810927-80185027ca84');
            background-size: cover;
            background-position: center;
            padding: 80px 20px;
            text-align: center;
            color: #fff;
        }
        header h1 {
            font-size: 2.5em;
            margin: 0;
            text-shadow: 1px 1px 4px #000;
        }
        header p {
            font-size: 1.2em;
            margin-top: 10px;
            text-shadow: 1px 1px 3px #000;
        }
        nav {
            background-color: #d2c4b2;
            text-align: center;
            padding: 10px;
        }
        nav a {
            margin: 0 15px;
            text-decoration: none;
            color: #3e3e3e;
            font-weight: bold;
        }
        nav a:hover {
            text-decoration: underline;
        }
        main {
            max-width: 800px;
            margin: 40px auto;
            padding: 0 20px;
        }
        .story, .story-form {
            background-color: #fff;
            border: 1px solid #ccc;
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 2px 2px 8px rgba(0,0,0,0.1);
        }
        .comments {
            margin-top: 20px;
        }
        .comments h3 {
            margin-bottom: 10px;
        }
        .comment {
            border-top: 1px solid #ddd;
            padding-top: 10px;
            margin-top: 10px;
        }
        .likes {
            margin-top: 10px;
            font-size: 0.9em;
            color: #555;
        }
        .rating {
            margin-top: 10px;
        }
        .star {
            font-size: 1.2em;
            color: #bbb;
            cursor: pointer;
        }
        .star.active {
            color: #f4b400;
        }
        footer {
            text-align: center;
            padding: 20px;
            background-color: #e2ded9;
            color: #555;
        }
        textarea, input[type="text"], select {
            width: 100%;
            padding: 10px;
            margin-top: 10px;
            border: 1px solid #bbb;
            border-radius: 6px;
            font-family: inherit;
        }
        button {
            margin-top: 10px;
            padding: 10px 20px;
            background-color: #8b6f47;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1em;
        }
        button:hover {
            background-color: #755b39;
        }
        .hidden {
            display: none;
        }
        .filters {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 20px;
        }
        .filters input, .filters select {
            flex: 1;
        }
    </style>
</head>
<body>
    <header>
        <h1>Histórias para Compartilhar</h1>
        <p>Um espaço rústico e acolhedor para publicar e ler histórias de todos os estilos</p>
    </header><nav>
    <a href="#" onclick="showSection('publicar')">Publicar</a>
    <a href="#" onclick="showSection('biblioteca')">Biblioteca</a>
</nav>

<main>
    <section id="publicar">
        <div class="story-form">
            <h2>Publique sua História</h2>
            <form id="storyForm">
                <input type="text" id="title" placeholder="Título da história" required>
                <textarea id="content" rows="6" placeholder="Escreva sua história aqui..." required></textarea>
                <select id="category">
                    <option value="">Escolha a categoria</option>
                    <option value="aventura">Aventura</option>
                    <option value="romance">Romance</option>
                    <option value="terror">Terror</option>
                    <option value="ficcao">Ficção</option>
                </select>
                <button type="submit">Publicar</button>
            </form>
        </div>
    </section>

    <section id="biblioteca" class="hidden">
        <h2>Biblioteca</h2>
        <div class="filters">
            <input type="text" id="searchBar" placeholder="Buscar por título...">
            <select id="filterCategory">
                <option value="">Todas as categorias</option>
                <option value="aventura">Aventura</option>
                <option value="romance">Romance</option>
                <option value="terror">Terror</option>
                <option value="ficcao">Ficção</option>
            </select>
        </div>
        <div id="stories"></div>
    </section>
</main>

<footer>
    &copy; 2025 Histórias para Compartilhar. Todos os direitos reservados. Criado por J.V.N.M.
</footer>

<script>
    const storyForm = document.getElementById('storyForm');
    const storiesDiv = document.getElementById('stories');
    const publicarSection = document.getElementById('publicar');
    const bibliotecaSection = document.getElementById('biblioteca');
    const searchBar = document.getElementById('searchBar');
    const filterCategory = document.getElementById('filterCategory');

    function showSection(id) {
        publicarSection.classList.add('hidden');
        bibliotecaSection.classList.add('hidden');
        document.getElementById(id).classList.remove('hidden');
    }

    function createStoryElement(title, content, category) {
        const storyElement = document.createElement('div');
        storyElement.classList.add('story');
        storyElement.setAttribute('data-title', title.toLowerCase());
        storyElement.setAttribute('data-category', category);
        storyElement.innerHTML = `
            <h2>${title}</h2>
            <p><em>Categoria: ${category}</em></p>
            <p>${content}</p>
            <div class="likes">Curtidas: <span class="like-count">0</span> <button class="like-btn">Curtir</button></div>
            <div class="rating">
                Avaliação:
                <span class="star">★</span>
                <span class="star">★</span>
                <span class="star">★</span>
                <span class="star">★</span>
                <span class="star">★</span>
            </div>
            <div class="comments">
                <h3>Comentários</h3>
                <div class="comment-list"></div>
                <form class="comment-form">
                    <input type="text" placeholder="Seu nome" required>
                    <textarea rows="2" placeholder="Deixe um comentário..." required></textarea>
                    <button type="submit">Comentar</button>
                </form>
            </div>
        `;
        return storyElement;
    }

    function saveStory(title, content, category) {
        const stories = JSON.parse(localStorage.getItem('stories') || '[]');
        stories.push({ title, content, category });
        localStorage.setItem('stories', JSON.stringify(stories));
    }

    function loadStories(titleFilter = '', categoryFilter = '') {
        const stories = JSON.parse(localStorage.getItem('stories') || '[]');
        storiesDiv.innerHTML = '';
        stories.forEach(({ title, content, category }) => {
            if (title.toLowerCase().includes(titleFilter.toLowerCase()) && 
                (categoryFilter === '' || category === categoryFilter)) {
                storiesDiv.appendChild(createStoryElement(title, content, category));
            }
        });
    }

    storyForm.addEventListener('submit', function(e) {
        e.preventDefault();
        const title = document.getElementById('title').value;
        const content = document.getElementById('content').value;
        const category = document.getElementById('category').value || 'sem categoria';
        saveStory(title, content, category);
        loadStories();
        showSection('biblioteca');
        storyForm.reset();
    });

    searchBar.addEventListener('input', function() {
        loadStories(this.value, filterCategory.value);
    });

    filterCategory.addEventListener('change', function() {
        loadStories(searchBar.value, this.value);
    });

    document.addEventListener('click', function(e) {
        if (e.target && e.target.classList.contains('like-btn')) {
            const likeCount = e.target.previousElementSibling;
            likeCount.textContent = parseInt(likeCount.textContent) + 1;
        }
        if (e.target && e.target.classList.contains('star')) {
            const stars = Array.from(e.target.parentElement.querySelectorAll('.star'));
            const index = stars.indexOf(e.target);
            stars.forEach((star, i) => {
                star.classList.toggle('active', i <= index);
            });
        }
    });

    document.addEventListener('submit', function(e) {
        if (e.target && e.target.classList.contains('comment-form')) {
            e.preventDefault();
            const form = e.target;
            const name = form.querySelector('input').value;
            const text = form.querySelector('textarea').value;
            const commentList = form.parentElement.querySelector('.comment-list');
            const comment = document.createElement('div');
            comment.classList.add('comment');
            comment.innerHTML = `<strong>${name}</strong><p>${text}</p>`;
            commentList.appendChild(comment);
            form.reset();
        }
    });

    loadStories();
</script>

</body>
</html>
