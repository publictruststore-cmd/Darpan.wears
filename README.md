<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Darpan Wears</title>
    <!-- Bootstrap 5 CDN -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Icons -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.5/font/bootstrap-icons.css" rel="stylesheet">
    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* Chatbot styles */
        #chatbot-btn {
            position: fixed;
            bottom: 30px;
            right: 30px;
            z-index: 1055;
            background: #007bff;
            color: #fff;
            border-radius: 50%;
            width: 56px;
            height: 56px;
            border: none;
            box-shadow: 0 2px 8px rgba(0,0,0,0.2);
            font-size: 2rem;
        }
        #chatbot-window {
            position: fixed;
            bottom: 90px;
            right: 30px;
            width: 320px;
            background: #fff;
            border: 1px solid #e5e5e5;
            border-radius: 12px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.2);
            display: none;
            z-index: 1056;
            flex-direction: column;
            overflow: hidden;
        }
        #chatbot-title {
            background: #007bff;
            color: #fff;
            padding: 10px;
            font-weight: 600;
        }
        #chatbot-history {
            padding: 10px;
            height: 230px;
            overflow-y: auto;
            font-size: 0.96rem;
        }
        #chatbot-input-row {
            border-top: 1px solid #eee;
            display: flex;
            align-items: center;
            background: #f8f9fa;
            padding: 5px;
        }
        #chatbot-msg {
            border: none;
            border-radius: 6px;
            flex: 1;
            margin-right: 8px;
            padding: 7px;
        }
        #chatbot-send {
            border: none;
            background: #007bff;
            color: #fff;
            border-radius: 4px;
            padding: 7px 16px;
        }
        .product-card {
            box-shadow: 0 2px 8px rgba(0,0,0,0.04);
        }
        /* Hide scrollbars on bot history (optional) */
        #chatbot-history::-webkit-scrollbar { display: none; }
        .category-btn {
            margin-right: 0.5rem;
            margin-bottom: 0.5rem;
        }
        .admin-btn {
            font-size: 1.1rem;
            margin-left: 0.7rem;
            cursor: pointer;
        }
    </style>
</head>
<body class="bg-light">

    <!-- Header & Nav -->
    <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm mb-3 sticky-top">
        <div class="container-fluid">
            <span class="navbar-brand fw-bold" style="font-size: 1.6rem;">
                The Darpan Wears
            </span>
            <a href="https://www.instagram.com/darpan_wears?igsh=a2pkYXhpajVwNnR3" target="_blank" class="mx-2">
                <i class="bi bi-instagram" style="font-size:1.7rem;color:#E1306C;"></i>
            </a>
            <button class="navbar-toggler" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div id="navbarNav" class="collapse navbar-collapse">
                <ul class="navbar-nav ms-auto me-3">
                    <li class="nav-item"><a class="nav-link category-link" data-cat="All" href="#">All</a></li>
                    <li class="nav-item"><a class="nav-link category-link" data-cat="Shirts" href="#">Shirts</a></li>
                    <li class="nav-item"><a class="nav-link category-link" data-cat="T-Shirts" href="#">T-Shirts</a></li>
                    <li class="nav-item"><a class="nav-link category-link" data-cat="Jeans" href="#">Jeans</a></li>
                    <li class="nav-item"><a class="nav-link category-link" data-cat="Shoes" href="#">Shoes</a></li>
                    <li class="nav-item"><a class="nav-link category-link" data-cat="Electronic Devices" href="#">Electronic Devices</a></li>
                </ul>
                <form class="d-flex" id="search-form" autocomplete="off">
                    <input class="form-control me-2" id="search-input" type="search" placeholder="Search products..." aria-label="Search">
                </form>
                <span class="admin-btn text-secondary" id="show-admin">
                    <i class="bi bi-person-lock"></i>
                </span>
            </div>
        </div>
    </nav>

    <!-- Category Buttons (For Mobile) -->
    <div class="container mb-2 d-lg-none">
        <div id="mobile-category" class="d-flex flex-wrap"></div>
    </div>

    <!-- Product Grid -->
    <div class="container" id="product-list">
        <div class="row" id="products-row"></div>
    </div>

    <!-- Modals Section -->

    <!-- Order Modal -->
    <div class="modal fade" id="orderModal" tabindex="-1" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <form id="orderForm">
            <div class="modal-header">
              <h5 class="modal-title">Order Product</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <div id="orderProductInfo" class="mb-2 small"></div>
                <div class="mb-2">
                    <label class="form-label">Full Name</label>
                    <input type="text" class="form-control" required id="orderName" maxlength="50">
                </div>
                <div class="mb-2">
                    <label class="form-label">Phone Number</label>
                    <input type="tel" class="form-control" required id="orderPhone" maxlength="15">
                </div>
                <div class="mb-2">
                    <label class="form-label">State</label>
                    <input type="text" class="form-control" required id="orderState" maxlength="30">
                </div>
                <div class="mb-2">
                    <label class="form-label">City</label>
                    <input type="text" class="form-control" required id="orderCity" maxlength="30">
                </div>
                <div class="mb-2">
                    <label class="form-label">Village Name</label>
                    <input type="text" class="form-control" required id="orderVillage" maxlength="30">
                </div>
                <div class="mb-2">
                    <label class="form-label">Pincode</label>
                    <input type="text" class="form-control" required id="orderPincode" maxlength="10">
                </div>
                <div>
                    <label class="form-label">Payment Method</label>
                    <div>
                        <button type="button" class="btn btn-outline-primary btn-sm me-2" id="codBtn">Cash on Delivery</button>
                        <button type="button" class="btn btn-outline-success btn-sm" id="onlineBtn">Online Payment</button>
                    </div>
                    <input type="hidden" required id="paymentMethod" value="">
                </div>
            </div>
            <div class="modal-footer">
              <button type="submit" class="btn btn-success">Place Order via WhatsApp</button>
            </div>
          </form>
        </div>
      </div>
    </div>

    <!-- Admin Login Modal -->
    <div class="modal fade" id="adminModal" tabindex="-1" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <form id="adminLoginForm" autocomplete="off">
            <div class="modal-header">
              <h5 class="modal-title">Admin Login</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
            </div>
            <div class="modal-body">
                <div class="mb-2">
                    <label>Password:</label>
                    <input type="password" class="form-control" id="adminPassword" maxlength="24" required>
                </div>
            </div>
            <div class="modal-footer">
              <button class="btn btn-primary" type="submit">Login</button>
            </div>
          </form>
        </div>
      </div>
    </div>

    <!-- Admin Panel Modal -->
    <div class="modal fade" id="adminPanelModal" tabindex="-1" aria-hidden="true">
      <div class="modal-dialog modal-lg modal-dialog-scrollable">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">
                Admin Panel
                <small class="ms-2 text-muted fs-6">(<span id="productCountAdmin">0</span> Products)</small>
            </h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
          </div>
          <div class="modal-body">

            <!-- Chart.js Graph -->
            <div class="mb-5">
                <canvas id="salesChart" height="60"></canvas>
            </div>

            <!-- Product Form -->
            <form id="productForm">
                <input type="hidden" id="editIdx" value="">
                <div class="row">
                    <div class="col-sm mb-2">
                        <label>Name</label>
                        <input type="text" id="prodName" class="form-control" maxlength="50" required>
                    </div>
                    <div class="col-sm mb-2">
                        <label>Description</label>
                        <input type="text" id="prodDesc" class="form-control" maxlength="64" required>
                    </div>
                </div>
                <div class="row">
                    <div class="col-sm mb-2">
                        <label>Category</label>
                        <select id="prodCat" class="form-select" required>
                            <option>Shirts</option>
                            <option>T-Shirts</option>
                            <option>Jeans</option>
                            <option>Shoes</option>
                            <option>Electronic Devices</option>
                        </select>
                    </div>
                    <div class="col-sm mb-2">
                        <label>Material</label>
                        <input type="text" id="prodMat" class="form-control" maxlength="30" required>
                    </div>
                    <div class="col-sm mb-2">
                        <label>Size</label>
                        <input type="text" id="prodSize" class="form-control" maxlength="10" required>
                    </div>
                </div>
                <div class="row">
                    <div class="col-sm mb-2">
                        <label>Original Price (₹)</label>
                        <input type="number" id="prodOrig" class="form-control" min="1" required>
                    </div>
                    <div class="col-sm mb-2">
                        <label>Selling Price (₹)</label>
                        <input type="number" id="prodSell" class="form-control" min="1" required>
                    </div>
                    <div class="col-sm mb-2">
                        <label>Special Price (₹)</label>
                        <input type="number" id="prodSpec" class="form-control" min="0">
                    </div>
                </div>
                <div class="mb-2">
                    <label>Image URL</label>
                    <input type="text" id="prodImg" class="form-control" maxlength="200" required>
                </div>
                <div class="text-end mb-2">
                    <button class="btn btn-success btn-sm" type="submit" id="addBtn">Add Product</button>
                    <button class="btn btn-warning btn-sm d-none" type="button" id="updateBtn">Update Product</button>
                    <button class="btn btn-secondary btn-sm d-none" type="button" id="cancelEditBtn">Cancel</button>
                </div>
            </form>
            <hr>
            <!-- Product Table (editable, deletable) -->
            <div class="table-responsive">
                <table class="table table-bordered table-hover table-sm align-middle" id="adminTable">
                    <thead class="table-light">
                        <tr>
                            <th>#</th>
                            <th>Name</th>
                            <th>Cat</th>
                            <th>Price</th>
                            <th>Size</th>
                            <th>Img</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody><!-- Dynamic --></tbody>
                </table>
            </div>
            <div class="text-muted small mt-2">
                <!-- Client-side passwords are not secure. Use server-side authentication for production. -->
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Footer -->
    <footer class="text-center text-muted mt-4 mb-1">
        © 2025 The Darpan Wears
    </footer>

    <!-- Chatbot Bubble -->
    <button id="chatbot-btn" aria-label="Chatbot">
        <i class="bi bi-chat-dots"></i>
    </button>
    <!-- Chatbot Window -->
    <div id="chatbot-window" class="d-flex flex-column">
        <div id="chatbot-title">
            <span>AI Chatbot</span>
            <button class="btn btn-sm btn-close float-end" id="chatbot-close"></button>
        </div>
        <div id="chatbot-history"></div>
        <div id="chatbot-input-row">
            <input type="text" id="chatbot-msg" class="form-control" placeholder="Type a message...">
            <button id="chatbot-send"><i class="bi bi-cursor"></i></button>
        </div>
    </div>

<!-- ========================== MAIN SCRIPT =============================== -->
<script>
// Util: Clean and extract usable image URLs from various formats
function cleanImageUrl(url) {
    // [img]https://...[/img]
    const bbcode = /[img](.+?)[/img]/i.exec(url);
    if (bbcode) return bbcode[1].trim();

    // Google Drive
    if (url.includes("drive.google.com")) {
        // shareable/viewer link to direct image link
        let imgIdMatch = url.match(//d/([a-zA-Z0-9_-]+)//);
        let imgId;
        if (imgIdMatch) imgId = imgIdMatch[1];
        else {
            let altMatch = url.match(/id=([a-zA-Z0-9_-]+)/);
            if (altMatch) imgId = altMatch[1];
        }
        if (imgId) return `https://drive.google.com/uc?export=view&id=${imgId}`;
    }
    // Direct image
    return url;
}

// ================= Data & Storage ==================
const PRODUCT_KEY = 'darpan_products';
// Sample products only if none exist
const SAMPLE_PRODUCTS = [
    {
        name: "Blue Denim Shirt",
        desc: "Trendy blue cotton denim for all seasons.",
        cat: "Shirts",
        orig: 1599,
        sell: 899,
        spec: 799,
        mat: "Cotton Denim",
        size: "M, L, XL",
        img: "https://images.unsplash.com/photo-1512436991641-6745cdb1723f"
    },
    {
        name: "Classic White Tee",
        desc: "Soft, stylish, everyday essential.",
        cat: "T-Shirts",
        orig: 699,
        sell: 349,
        spec: "",
        mat: "Pure Cotton",
        size: "S, M, L, XL",
        img: "[img]https://images.unsplash.com/photo-1484517186945-119c527b2b83[/img]"
    },
    {
        name: "Premium Slim Jeans",
        desc: "Stretch jeans, perfect fit and comfort.",
        cat: "Jeans",
        orig: 2399,
        sell: 1399,
        spec: 1299,
        mat: "Elastane Denim",
        size: "32, 34, 36",
        img: "https://drive.google.com/file/d/1Mub6Jh1uKM_crZ6Hz33i85clp5k2osv6/view?usp=sharing"
    }
];

// Retrieve or init products
function getProducts() {
    let data = localStorage.getItem(PRODUCT_KEY);
    return data ? JSON.parse(data) : (localStorage.setItem(PRODUCT_KEY, JSON.stringify(SAMPLE_PRODUCTS)), SAMPLE_PRODUCTS.slice());
}
function saveProducts(arr) {
    localStorage.setItem(PRODUCT_KEY, JSON.stringify(arr));
}

// =========== Product Rendering ============
function renderProducts(arr) {
    const row = document.getElementById('products-row');
    row.innerHTML = '';
    arr.forEach((prod, i) => {
        let imgSrc = cleanImageUrl(prod.img);
        let priceSection =
        `<span class="text-decoration-line-through text-muted">₹${prod.orig}</span>
         <span class="fw-bold text-primary mx-1">₹${prod.sell}</span>`;
        if (prod.spec && prod.spec > 0) priceSection += `<span class="badge bg-danger ms-2">₹${prod.spec}</span>`;
        row.innerHTML += `
        <div class="col-12 col-md-6 mb-4 d-flex align-items-stretch">
            <div class="card product-card w-100">
                <img src="${imgSrc}" class="card-img-top" style="height:210px;object-fit:cover" alt="${prod.name}">
                <div class="card-body pb-1">
                    <h6 class="fw-bold">${prod.name}</h6>
                    <div class="small text-muted mb-1">${prod.cat}</div>
                    <div class="small mb-2">${prod.desc}</div>
                    <div class="mb-1">${priceSection}</div>
                    <div class="small text-muted">Material: ${prod.mat} | Size: ${prod.size}</div>
                </div>
                <div class="card-footer bg-white border-0 text-end">
                    <button class="btn btn-sm btn-success" onclick="openOrderModal(${i})">Order Now</button>
                </div>
            </div>
        </div>`;
    });
}

// =============== Search and Filter ===========
function applySearchFilter() {
    const val = document.getElementById('search-input').value.trim().toLowerCase();
    let cat = document.querySelector('.category-link.active')?.getAttribute('data-cat') || "All";
    let prods = getProducts().filter(p => {
        let matched = (!val || p.name.toLowerCase().includes(val) || p.cat.toLowerCase().includes(val));
        let catMatch = (cat === "All" || p.cat === cat);
        return matched && catMatch;
    });
    renderProducts(prods);
}
document.getElementById('search-input').addEventListener('input', applySearchFilter);
document.querySelectorAll('.category-link').forEach(el => {
    el.addEventListener('click', function(e) {
        e.preventDefault();
        document.querySelectorAll('.category-link').forEach(n=>n.classList.remove('active'));
        this.classList.add('active');
        applySearchFilter();
    });
});
window.onload = function() {
    renderProducts(getProducts());
    // Mobile category buttons
    let cats = ["All","Shirts","T-Shirts","Jeans","Shoes","Electronic Devices"];
    let mobileCat = document.getElementById('mobile-category');
    mobileCat.innerHTML = cats.map(c => `<button class="btn btn-outline-secondary category-btn" onclick="mobileCatSelect('${c}')">${c}</button>`).join('');
}

// Mobile filter function
function mobileCatSelect(cat) {
    document.querySelectorAll('.category-link').forEach(n=>n.classList.remove('active'));
    let btn = Array.from(document.querySelectorAll('.category-link')).find(n=>n.dataset.cat===cat);
    btn?.classList.add('active');
    applySearchFilter();
}

// =========== Order Modal & WhatsApp ============
let currentOrderIdx = null;

function openOrderModal(idx) {
    currentOrderIdx = idx;
    const prod = getProducts()[idx];
    let imgSrc = cleanImageUrl(prod.img);
    document.getElementById('orderProductInfo').innerHTML = `
        <strong>Product:</strong> ${prod.name}<br>
        <strong>Price:</strong> ₹${prod.spec && prod.spec > 0 ? prod.spec : prod.sell}<br>
        <img src="${imgSrc}" style="height:48px;width:auto;border-radius:6px;"><br>`;
    document.getElementById('orderForm').reset();
    document.getElementById('paymentMethod').value = "";
    new bootstrap.Modal(document.getElementById('orderModal')).show();
}
document.getElementById('codBtn').onclick = function() {
    document.getElementById('paymentMethod').value = "Cash on Delivery";
    this.classList.add('btn-primary');
    this.classList.remove('btn-outline-primary');
    document.getElementById('onlineBtn').classList.remove('btn-success');
    document.getElementById('onlineBtn').classList.add('btn-outline-success');
};
document.getElementById('onlineBtn').onclick = function() {
    document.getElementById('paymentMethod').value = "Online Payment";
    this.classList.add('btn-success');
    this.classList.remove('btn-outline-success');
    document.getElementById('codBtn').classList.remove('btn-primary');
    document.getElementById('codBtn').classList.add('btn-outline-primary');
};

document.getElementById('orderForm').onsubmit = function(e) {
    e.preventDefault();
    const prod = getProducts()[currentOrderIdx];
    let size = prod.size;
    let finalP = prod.spec && prod.spec > 0 ? prod.spec : prod.sell;
    let imgUrl = cleanImageUrl(prod.img);
    let message =
`🚨 Alert: Darpan Wears — New Order!
🛍 Product: ${prod.name}
📏 Size: ${size}
💰 Final Price: ₹${finalP}
📸 Product Image: ${imgUrl}
👤 Customer Name: ${document.getElementById('orderName').value}
📞 Phone: ${document.getElementById('orderPhone').value}
🏠 Address: ${document.getElementById('orderVillage').value}, ${document.getElementById('orderCity').value}, ${document.getElementById('orderState').value} - ${document.getElementById('orderPincode').value}
💳 Payment Method: ${document.getElementById('paymentMethod').value}`;
    let waNum = '919332307996';
    let msg = encodeURIComponent(message);
    // Detect WA Business if present
    let waUrl = `https://wa.me/${waNum}?text=${msg}`;
    window.open(waUrl,'_blank');
    bootstrap.Modal.getInstance(document.getElementById('orderModal')).hide();
};

// ============== Admin Panel =============
const ADMIN_PASS = 'darpan2025';
// Show login modal
document.getElementById('show-admin').onclick = function() {
    document.getElementById('adminLoginForm').reset();
    new bootstrap.Modal(document.getElementById('adminModal')).show();
};
document.getElementById('adminLoginForm').onsubmit = function(e) {
    e.preventDefault();
    const pw = document.getElementById('adminPassword').value;
    if (pw === ADMIN_PASS) {
        bootstrap.Modal.getInstance(document.getElementById('adminModal')).hide();
        showAdminPanel();
    } else {
        alert('Incorrect password!');
    }
};
// ========= Admin Panel Content ==============
function showAdminPanel() {
    document.getElementById('productForm').reset();
    document.getElementById('editIdx').value = "";
    document.getElementById('addBtn').classList.remove('d-none');
    document.getElementById('updateBtn').classList.add('d-none');
    document.getElementById('cancelEditBtn').classList.add('d-none');
    renderAdminProducts();
    new bootstrap.Modal(document.getElementById('adminPanelModal')).show();
    setTimeout(renderSalesChart, 150); // ensure modal open before rendering chart
}
// Admin add/edit/delete
document.getElementById('productForm').onsubmit = function(e){
    e.preventDefault();
    let products = getProducts();
    let obj = {
        name: document.getElementById('prodName').value,
        desc: document.getElementById('prodDesc').value,
        cat: document.getElementById('prodCat').value,
        orig: +document.getElementById('prodOrig').value,
        sell: +document.getElementById('prodSell').value,
        spec: Number(document.getElementById('prodSpec').value) || "",
        mat: document.getElementById('prodMat').value,
        size: document.getElementById('prodSize').value,
        img: document.getElementById('prodImg').value
    };
    if (!document.getElementById('editIdx').value) {
        products.push(obj);
    } else {
        let i = +document.getElementById('editIdx').value;
        products[i] = obj;
    }
    saveProducts(products);
    renderProducts(products);
    renderAdminProducts();
    this.reset();
    document.getElementById('editIdx').value = "";
    document.getElementById('addBtn').classList.remove('d-none');
    document.getElementById('updateBtn').classList.add('d-none');
    document.getElementById('cancelEditBtn').classList.add('d-none');
};
// Admin update/cancel
document.getElementById('updateBtn').onclick = document.getElementById('cancelEditBtn').onclick = function(){
    document.getElementById('productForm').reset();
    document.getElementById('editIdx').value = "";
    this.classList.add('d-none');
    document.getElementById('addBtn').classList.remove('d-none');
    document.getElementById('updateBtn').classList.add('d-none');
    document.getElementById('cancelEditBtn').classList.add('d-none');
};

function renderAdminProducts() {
    let products = getProducts();
    let tb = document.querySelector('#adminTable tbody');
    tb.innerHTML = '';
    products.forEach((p,i) => {
        let imgSrc = cleanImageUrl(p.img);
        tb.innerHTML += `<tr>
            <td>${i+1}</td>
            <td>${p.name}</td>
            <td>${p.cat}</td>
            <td class="small">₹${p.sell}</td>
            <td>${p.size}</td>
            <td>
                <img src="${imgSrc}" style="height:32px;width:auto;border-radius:4px">
            </td>
            <td>
                <button class="btn btn-sm btn-warning me-1" onclick="adminEdit(${i})">Edit</button>
                <button class="btn btn-sm btn-danger" onclick="adminDelete(${i})">Delete</button>
            </td>
        </tr>`;
    });
    document.getElementById('productCountAdmin').innerText = products.length;
}
function adminEdit(idx){
    let p = getProducts()[idx];
    document.getElementById('prodName').value = p.name;
    document.getElementById('prodDesc').value = p.desc;
    document.getElementById('prodCat').value = p.cat;
    document.getElementById('prodOrig').value = p.orig;
    document.getElementById('prodSell').value = p.sell;
    document.getElementById('prodSpec').value = p.spec;
    document.getElementById('prodMat').value = p.mat;
    document.getElementById('prodSize').value = p.size;
    document.getElementById('prodImg').value = p.img;
    document.getElementById('editIdx').value = idx;
    document.getElementById('addBtn').classList.add('d-none');
    document.getElementById('updateBtn').classList.remove('d-none');
    document.getElementById('cancelEditBtn').classList.remove('d-none');
}
function adminDelete(idx){
    if (confirm('Delete this product?')) {
        let arr = getProducts();
        arr.splice(idx,1);
        saveProducts(arr);
        renderProducts(arr);
        renderAdminProducts();
    }
}

// Sales analysis: Counts by category
function renderSalesChart() {
    let products = getProducts();
    let cats = {};
    products.forEach(p=>{
        cats[p.cat] = (cats[p.cat]||0) + 1;
    });
    let ctx = document.getElementById('salesChart').getContext('2d');
    if (window.salesChartInst) window.salesChartInst.destroy();
    window.salesChartInst = new Chart(ctx, {
        type: 'bar',
        data: {
            labels: Object.keys(cats),
            datasets: [{
                label: '# Products',
                data: Object.values(cats),
                backgroundColor: 'rgba(54,162,235,0.6)'
            }]
        },
        options: {
            responsive: true, plugins:{legend:{display:false}},
            scales:{y:{beginAtZero:true, precision:0}}
        }
    });
}

// =========== Chatbot (Offline) ===========
const BOT_RESP = [
    { q:/hello|hi|hey/i, a:"Hi there! Welcome to The Darpan Wears 👕" },
    { q:/order|buy|purchase/i, a:"Sure! You can order directly by clicking the Order Now button on any product." },
    { q:/payment/i, a:"We support Cash on Delivery and Online Payment." },
    { q:/where.*order/i, a:"Our team will contact you after your order confirmation." },
    { q:/thank/i, a:"You're welcome! 😊" }
];
function botReply(msg) {
    let found = BOT_RESP.find(r => r.q.test(msg));
    return found ? found.a : "Sorry, I can't answer that now. Please try again!";
}
const cbBtn = document.getElementById('chatbot-btn');
const cbWin = document.getElementById('chatbot-window');
const cbClose = document.getElementById('chatbot-close');
const cbHist = document.getElementById('chatbot-history');
const cbInput = document.getElementById('chatbot-msg');
const cbSend = document.getElementById('chatbot-send');
cbBtn.onclick = () => {
    cbWin.style.display = 'flex'; setTimeout(()=>cbWin.scrollIntoView({behavior:'smooth'}),150);
};
cbClose.onclick = () => cbWin.style.display="none";
cbSend.onclick = sendBotMsg; cbInput.onkeydown = e=>{if(e.key==='Enter') sendBotMsg()};
function sendBotMsg(){
    let userMsg = cbInput.value.trim();
    if (!userMsg) return;
    appendBotMsg("You", userMsg);
    let reply = botReply(userMsg);
    setTimeout(()=>appendBotMsg("Bot", reply), 600);
    cbInput.value='';
}
function appendBotMsg(who, msg){
    cbHist.innerHTML += `<div><strong>${who}:</strong> ${msg}</div>`;
    cbHist.scrollTop = cbHist.scrollHeight;
}

// =============== Responsive adjustments ============
window.addEventListener('resize', function() {
    renderProducts(getProducts());
});
</script>
<!-- 
© 2025 The Darpan Wears -->
<!-- Developer: Darpan Karki -->
<!-- Note: This is a front-end demo only. For real stores, use server-side authentication and databases. -->
</body>
</html>
