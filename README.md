# tasks2.4
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Multi-Project Demo</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: Arial, sans-serif; }
    body { background: #f4f4f4; color: #333; }
    header, section { padding: 20px; margin-bottom: 20px; }
    nav { background: #222; color: white; display: flex; justify-content: space-around; padding: 15px 0; }
    nav a { color: white; text-decoration: none; padding: 5px 10px; }
    h1, h2 { margin-bottom: 10px; }

    /* Portfolio Section */
    #about, #projects, #contact { background: white; border-radius: 5px; padding: 20px; margin: 10px auto; width: 90%; max-width: 800px; }
    .project { margin: 10px 0; padding: 10px; border: 1px solid #ddd; }

    /* To-Do App */
    #todo-app { background: white; padding: 20px; border-radius: 5px; width: 90%; max-width: 600px; margin: auto; }
    #todo-list li { display: flex; justify-content: space-between; padding: 5px 0; }
    #todo-input { width: 80%; padding: 8px; }
    #add-btn { padding: 8px; }

    /* Product Listing */
    #products { background: white; padding: 20px; border-radius: 5px; width: 90%; max-width: 800px; margin: auto; }
    .product-card { border: 1px solid #ddd; padding: 10px; margin: 10px 0; border-radius: 4px; }
    select, input[type="text"] { margin: 10px 0; padding: 6px; width: 100%; }
  </style>
</head>
<body>

  <nav>
    <a href="#about">About</a>
    <a href="#projects">Projects</a>
    <a href="#contact">Contact</a>
    <a href="#todo-app">To-Do</a>
    <a href="#products">Products</a>
  </nav>

  <!-- Portfolio Sections -->
  <section id="about">
    <h2>About Me</h2>
    <p>Hello! I'm a web developer passionate about creating interactive and user-friendly applications. I love working with HTML, CSS, and JavaScript.</p>
  </section>

  <section id="projects">
    <h2>My Projects</h2>
    <div class="project"><strong>Portfolio Website</strong> - Showcasing my work.</div>
    <div class="project"><strong>To-Do List App</strong> - Save tasks with local storage.</div>
    <div class="project"><strong>Product Filter</strong> - Filter and sort products dynamically.</div>
  </section>

  <section id="contact">
    <h2>Contact</h2>
    <p>Email: developer@example.com</p>
  </section>

  <!-- To-Do List App -->
  <section id="todo-app">
    <h2>To-Do List</h2>
    <input type="text" id="todo-input" placeholder="Add a new task..." />
    <button id="add-btn">Add</button>
    <ul id="todo-list"></ul>
  </section>

  <!-- Product Listing -->
  <section id="products">
    <h2>Product Listing</h2>
    <label>Filter by Category:</label>
    <select id="category-filter">
      <option value="All">All</option>
      <option value="Electronics">Electronics</option>
      <option value="Books">Books</option>
    </select>

    <label>Sort by:</label>
    <select id="sort-option">
      <option value="none">None</option>
      <option value="price-asc">Price Low to High</option>
      <option value="price-desc">Price High to Low</option>
    </select>

    <div id="product-list"></div>
  </section>

  <script>
    // To-Do App
    const todoInput = document.getElementById('todo-input');
    const addBtn = document.getElementById('add-btn');
    const todoList = document.getElementById('todo-list');

    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];

    function renderTasks() {
      todoList.innerHTML = "";
      tasks.forEach((task, index) => {
        const li = document.createElement('li');
        li.textContent = task;
        const del = document.createElement('button');
        del.textContent = "❌";
        del.onclick = () => {
          tasks.splice(index, 1);
          localStorage.setItem('tasks', JSON.stringify(tasks));
          renderTasks();
        };
        li.appendChild(del);
        todoList.appendChild(li);
      });
    }

    addBtn.onclick = () => {
      const task = todoInput.value.trim();
      if (task) {
        tasks.push(task);
        localStorage.setItem('tasks', JSON.stringify(tasks));
        renderTasks();
        todoInput.value = "";
      }
    };

    renderTasks();

    // Product Listing
    const products = [
      { name: "Smartphone", category: "Electronics", price: 500 },
      { name: "Laptop", category: "Electronics", price: 1000 },
      { name: "Novel", category: "Books", price: 15 },
      { name: "Biography", category: "Books", price: 25 }
    ];

    const productList = document.getElementById('product-list');
    const categoryFilter = document.getElementById('category-filter');
    const sortOption = document.getElementById('sort-option');

    function displayProducts(filteredProducts) {
      productList.innerHTML = "";
      filteredProducts.forEach(p => {
        const div = document.createElement('div');
        div.className = "product-card";
        div.innerHTML = <strong>${p.name}</strong><br>Category: ${p.category}<br>Price: $${p.price};
        productList.appendChild(div);
      });
    }

    function filterAndSort() {
      let filtered = [...products];
      const category = categoryFilter.value;
      const sort = sortOption.value;

      if (category !== "All") {
        filtered = filtered.filter(p => p.category === category);
      }

      if (sort === "price-asc") {
        filtered.sort((a, b) => a.price - b.price);
      } else if (sort === "price-desc") {
        filtered.sort((a, b) => b.price - a.price);
      }

      displayProducts(filtered);
    }

    categoryFilter.onchange = filterAndSort;
    sortOption.onchange = filterAndSort;

    filterAndSort();
  </script>
</body>
</html>
