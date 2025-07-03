<!DOCTYPE html>
<html lang="en">
<head>
    <base target="_self">
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GAG Shop - Buy, Sell, Trade</title>
    <meta name="description" content="Platform for buying, selling and trading items">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-auth.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-firestore.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: "#4F46E5",
                        secondary: "#10B981"
                    }
                }
            }
        }
        // Firebase configuration
        const firebaseConfig = {
            apiKey: "AIzaSyDEXAMPLEEXAMPLEEXAMPLEEXAMPLE",
            authDomain: "gag-shop.firebaseapp.com",
            projectId: "gag-shop",
            storageBucket: "gag-shop.appspot.com",
            messagingSenderId: "123456789012",
            appId: "1:123456789012:web:abcdef1234567890abcdef"
        };
        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();
        const db = firebase.firestore();
        const provider = new firebase.auth.GoogleAuthProvider();
        // Admin token
        const ADMIN_TOKEN = "gag-Admin-shop";
    </script>
</head>
<body class="min-h-screen bg-gray-100">
    <header class="bg-white shadow-sm">
        <nav class="container mx-auto px-4 py-4 flex justify-between items-center">
            <h1 class="text-2xl font-bold text-primary">GAG Shop</h1>
            <div id="auth-section" class="flex items-center space-x-4">
                <button id="login-btn" class="px-4 py-2 bg-primary text-white rounded hover:bg-opacity-90 transition">
                    Login with Google
                </button>
                <button id="logout-btn" class="hidden px-4 py-2 bg-red-500 text-white rounded hover:bg-opacity-90 transition">
                    Logout
                </button>
            </div>
        </nav>
    </header>
    <main class="container mx-auto px-4 py-8">
        <div id="admin-section" class="hidden mb-8 p-6 bg-white rounded-lg shadow">
            <h2 class="text-xl font-semibold mb-4">Admin Panel</h2>
            <div class="mb-4">
                <input type="text" id="admin-token" placeholder="Enter admin token" class="w-full p-2 border rounded">
                <button id="verify-token" class="mt-2 px-4 py-2 bg-primary text-white rounded hover:bg-opacity-90 transition">
                    Verify Token
                </button>
            </div>
            <div id="add-product-form" class="hidden">
                <h3 class="text-lg font-medium mb-2">Add New Product</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <input type="text" id="product-name" placeholder="Product name" class="p-2 border rounded">
                    <input type="number" id="product-price" placeholder="Price" class="p-2 border rounded">
                    <textarea id="product-description" placeholder="Description" class="p-2 border rounded md:col-span-2"></textarea>
                    <input type="text" id="product-category" placeholder="Category" class="p-2 border rounded">
                    <input type="text" id="product-image" placeholder="Image URL" class="p-2 border rounded">
                </div>
                <button id="submit-product" class="mt-4 px-4 py-2 bg-secondary text-white rounded hover:bg-opacity-90 transition">
                    Add Product
                </button>
            </div>
        </div>
        <div id="products-section" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
            <!-- Products will be loaded here -->
        </div>
        <div id="requests-section" class="mt-12 hidden">
            <h2 class="text-xl font-semibold mb-4">Your Requests</h2>
            <div id="requests-list" class="space-y-4">
                <!-- Requests will be loaded here -->
            </div>
        </div>
    </main>
    <footer class="bg-gray-800 text-white py-8">
        <div class="container mx-auto px-4">
            <div class="flex flex-col md:flex-row justify-between items-center">
                <div class="mb-4 md:mb-0">
                    <h2 class="text-xl font-bold">GAG Shop</h2>
                    <p class="text-gray-400">Buy, Sell, Trade</p>
                </div>
                <div class="flex space-x-4">
                    <a href="#" class="hover:text-primary transition">Terms</a>
                    <a href="#" class="hover:text-primary transition">Privacy</a>
                    <a href="#" class="hover:text-primary transition">Contact</a>
                </div>
            </div>
        </div>
    </footer>
    <script>
        // DOM elements
        const loginBtn = document.getElementById('login-btn');
        const logoutBtn = document.getElementById('logout-btn');
        const authSection = document.getElementById('auth-section');
        const adminSection = document.getElementById('admin-section');
        const productsSection = document.getElementById('products-section');
        const requestsSection = document.getElementById('requests-section');
        const verifyTokenBtn = document.getElementById('verify-token');
        const adminTokenInput = document.getElementById('admin-token');
        const addProductForm = document.getElementById('add-product-form');
        const submitProductBtn = document.getElementById('submit-product');
        // Current user data
        let currentUser = null;
        let isAdmin = false;
        // Auth state listener
        auth.onAuthStateChanged(user => {
            if (user) {
                currentUser = user;
                loginBtn.classList.add('hidden');
                logoutBtn.classList.remove('hidden');
                adminSection.classList.remove('hidden');
                requestsSection.classList.remove('hidden');
                loadProducts();
                loadUserRequests();
            } else {
                currentUser = null;
                loginBtn.classList.remove('hidden');
                logoutBtn.classList.add('hidden');
                adminSection.classList.add('hidden');
                requestsSection.classList.add('hidden');
                productsSection.innerHTML = '';
            }
        });
        // Event listeners
        loginBtn.addEventListener('click', signInWithGoogle);
        logoutBtn.addEventListener('click', signOut);
        verifyTokenBtn.addEventListener('click', verifyAdminToken);
        submitProductBtn.addEventListener('click', addProduct);
        // Functions
        function signInWithGoogle() {
            auth.signInWithPopup(provider)
                .then(result => {
                    Swal.fire({
                        title: "Success!",
                        text: "You're now logged in",
                        icon: "success"
                    });
                })
                .catch(error => {
                    Swal.fire({
                        title: "Error!",
                        text: error.message,
                        icon: "error"
                    });
                });
        }
        function signOut() {
            auth.signOut()
                .then(() => {
                    Swal.fire({
                        title: "Logged out",
                        text: "You've been successfully logged out",
                        icon: "success"
                    });
                })
                .catch(error => {
                    Swal.fire({
                        title: "Error!",
                        text: error.message,
                        icon: "error"
                    });
                });
        }
        function verifyAdminToken() {
            if (adminTokenInput.value === ADMIN_TOKEN) {
                isAdmin = true;
                addProductForm.classList.remove('hidden');
                Swal.fire({
                    title: "Success!",
                    text: "Admin privileges granted",
                    icon: "success"
                });
            } else {
                isAdmin = false;
                addProductForm.classList.add('hidden');
                Swal.fire({
                    title: "Error!",
                    text: "Invalid admin token",
                    icon: "error"
                });
            }
        }
        function addProduct() {
            if (!isAdmin) return;
            const name = document.getElementById('product-name').value;
            const price = document.getElementById('product-price').value;
            const description = document.getElementById('product-description').value;
            const category = document.getElementById('product-category').value;
            const image = document.getElementById('product-image').value;
            if (!name || !price || !description || !category) {
                Swal.fire({
                    title: "Error!",
                    text: "Please fill all required fields",
                    icon: "error"
                });
                return;
            }
            db.collection('products').add({
                name,
                price: parseFloat(price),
                description,
                category,
                image: image || "https://picsum.photos/400?random=" + Math.floor(Math.random() * 100),
                createdAt: firebase.firestore.FieldValue.serverTimestamp()
            })
            .then(() => {
                Swal.fire({
                    title: "Success!",
                    text: "Product added successfully",
                    icon: "success"
                });
                document.getElementById('product-name').value = '';
                document.getElementById('product-price').value = '';
                document.getElementById('product-description').value = '';
                document.getElementById('product-category').value = '';
                document.getElementById('product-image').value = '';
                loadProducts();
            })
            .catch(error => {
                Swal.fire({
                    title: "Error!",
                    text: error.message,
                    icon: "error"
                });
            });
        }
        function loadProducts() {
            productsSection.innerHTML = '';
            db.collection('products').orderBy('createdAt', 'desc').get()
                .then(snapshot => {
                    snapshot.forEach(doc => {
                        const product = doc.data();
                        const productCard = document.createElement('div');
                        productCard.className = 'bg-white rounded-lg shadow overflow-hidden';
                        productCard.innerHTML = `
                            <img src="${product.image}" alt="${product.name}" class="w-full h-48 object-cover" loading="lazy">
                            <div class="p-4">
                                <h3 class="font-semibold text-lg">${product.name}</h3>
                                <p class="text-gray-600 mt-1">${product.category}</p>
                                <p class="text-primary font-bold mt-2">$${product.price.toFixed(2)}</p>
                                <p class="text-gray-700 mt-2">${product.description}</p>
                                <div class="mt-4 flex justify-between items-center">
                                    <button onclick="sendRequest('${doc.id}', '${product.name}')" class="px-3 py-1 bg-primary text-white rounded hover:bg-opacity-90 transition">
                                        Send Request
                                    </button>
                                    <button onclick="showSellerInfo('${doc.id}')" class="px-3 py-1 bg-gray-200 rounded hover:bg-gray-300 transition">
                                        Contact Seller
                                    </button>
                                </div>
                            </div>
                        `;
                        productsSection.appendChild(productCard);
                    });
                })
                .catch(error => {
                    console.error("Error loading products: ", error);
                });
        }
        function sendRequest(productId, productName) {
            if (!currentUser) {
                Swal.fire({
                    title: "Error!",
                    text: "You need to login first",
                    icon: "error"
                });
                return;
            }
            Swal.fire({
                title: "Send Request",
                input: "textarea",
                inputLabel: `Your request for ${productName}`,
                inputPlaceholder: "Type your message here...",
                showCancelButton: true,
                confirmButtonText: "Send",
                preConfirm: (message) => {
                    if (!message) {
                        Swal.showValidationMessage("Please enter your message");
                        return false;
                    }
                    return db.collection('requests').add({
                        productId,
                        productName,
                        buyerId: currentUser.uid,
                        buyerEmail: currentUser.email,
                        buyerName: currentUser.displayName,
                        message,
                        status: "pending",
                        createdAt: firebase.firestore.FieldValue.serverTimestamp()
                    });
                }
            }).then(result => {
                if (result.isConfirmed) {
                    Swal.fire({
                        title: "Success!",
                        text: "Your request has been sent",
                        icon: "success"
                    });
                    loadUserRequests();
                }
            });
        }
        function loadUserRequests() {
            if (!currentUser) return;
            const requestsList = document.getElementById('requests-list');
            requestsList.innerHTML = '';
            db.collection('requests')
                .where('buyerId', '==', currentUser.uid)
                .orderBy('createdAt', 'desc')
                .get()
                .then(snapshot => {
                    if (snapshot.empty) {
                        requestsList.innerHTML = '<p class="text-gray-500">You have no requests yet</p>';
                        return;
                    }
                    snapshot.forEach(doc => {
                        const request = doc.data();
                        const requestItem = document.createElement('div');
                        requestItem.className = 'bg-white p-4 rounded-lg shadow';
                        requestItem.innerHTML = `
                            <div class="flex justify-between items-start">
                                <div>
                                    <h3 class="font-medium">${request.productName}</h3>
                                    <p class="text-gray-600 text-sm mt-1">${request.message}</p>
                                    <p class="text-sm mt-2">
                                        Status: <span class="font-semibold ${request.status === 'approved' ? 'text-green-500' : request.status === 'rejected' ? 'text-red-500' : 'text-yellow-500'}">${request.status}</span>
                                    </p>
                                </div>
                                <button onclick="deleteRequest('${doc.id}')" class="text-red-500 hover:text-red-700">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        `;
                        requestsList.appendChild(requestItem);
                    });
                })
                .catch(error => {
                    console.error("Error loading requests: ", error);
                });
        }
        function deleteRequest(requestId) {
            Swal.fire({
                title: "Are you sure?",
                text: "You won't be able to revert this!",
                icon: "warning",
                showCancelButton: true,
                confirmButtonColor: "#d33",
                confirmButtonText: "Yes, delete it!"
            }).then(result => {
                if (result.isConfirmed) {
                    db.collection('requests').doc(requestId).delete()
                        .then(() => {
                            Swal.fire({
                                title: "Deleted!",
                                text: "Your request has been deleted",
                                icon: "success"
                            });
                            loadUserRequests();
                        })
                        .catch(error => {
                            Swal.fire({
                                title: "Error!",
                                text: error.message,
                                icon: "error"
                            });
                        });
                }
            });
        }
        function showSellerInfo(productId) {
            // In a real implementation, this would fetch seller info from the product document
            Swal.fire({
                title: "Contact Seller",
                html: "Please use the request system to contact the seller about this product",
                icon: "info"
            });
        }
        // Initialize the page
        document.addEventListener('DOMContentLoaded', () => {
            // Check if user is admin on page load
            if (currentUser) {
                // In a real app, you might check the user's role in Firestore
            }
        });
    </script>
</body>
</html>
