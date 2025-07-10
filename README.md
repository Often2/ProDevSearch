<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>proDevSearch</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* Custom styles for the sleek search engine */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #121212;
            color: #e0e0e0;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            transition: background-color 0.3s, color 0.3s;
        }

        #background-slideshow {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -2;
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            transition: background-image 1.5s ease-in-out;
        }

        #background-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(18, 18, 18, 0.6); /* Less dark overlay */
            z-index: -1;
            transition: background-color 0.3s;
        }

        .search-container {
            transition: all 0.3s ease-in-out;
        }

        .search-bar-wrapper {
            position: relative;
            width: 100%;
            max-width: 600px;
        }

        /* The glowing effect for the search bar */
        .search-bar-glow {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 105%;
            height: 140%;
            background: radial-gradient(ellipse at center, rgba(52, 152, 219, 0.3) 0%, rgba(52, 152, 219, 0) 70%);
            border-radius: 9999px;
            z-index: -1;
            pointer-events: none;
            transition: all 0.3s ease;
        }

        .search-input:focus-within + .search-bar-glow {
            transform: translate(-50%, -50%) scale(1.05);
            background: radial-gradient(ellipse at center, rgba(52, 152, 219, 0.4) 0%, rgba(52, 152, 219, 0) 70%);
        }

        .search-input {
            background-color: #282828;
            border: 1px solid #444;
            transition: background-color 0.3s, border-color 0.3s, box-shadow 0.3s;
        }

        .search-input:focus-within {
            border-color: #3498db;
            box-shadow: 0 0 0 1px #3498db;
        }

        .search-btn, .lucky-btn {
            background-color: #333;
            border: 1px solid #444;
            transition: background-color 0.2s ease, border-color 0.2s ease;
        }

        .search-btn:hover, .lucky-btn:hover {
            background-color: #444;
            border-color: #555;
        }

        .filter-btn {
            color: #aaa;
            transition: color 0.2s ease, border-bottom-color 0.2s ease;
            border-bottom: 2px solid transparent;
        }

        .filter-btn.active, .filter-btn:hover {
            color: #3498db;
        }
        
        .filter-btn.active {
             border-bottom-color: #3498db;
        }
        
        .svg-logo {
             width: 160px;
             height: auto;
             cursor: pointer;
             filter: drop-shadow(0 0 10px rgba(52, 152, 219, 0.6));
             transition: filter 0.3s ease;
        }

        .svg-logo:hover {
            filter: drop-shadow(0 0 15px rgba(52, 152, 219, 0.8));
        }

        .page-content {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start; /* Align content to the top */
            width: 100%;
            padding: 2rem 1rem;
            background-color: rgba(18, 18, 18, 0.3); /* Lighter content background */
            border-radius: 1rem;
            margin: 1rem;
        }
        
        main#search-page.page-content {
            background-color: transparent;
            border-radius: 0;
            margin: 0;
        }
        
        .cart-item-remove-btn {
            transition: color 0.2s ease;
        }
        .cart-item-remove-btn:hover {
            color: #EA4335;
        }

        /* Light Mode Styles */
        body.light-mode #background-overlay {
            background-color: rgba(248, 250, 252, 0.7); /* Less dark overlay for light mode */
        }
        body.light-mode {
            color: #0f172a;
        }
        body.light-mode header,
        body.light-mode footer,
        body.light-mode .bg-gray-800 {
            background-color: rgba(226, 232, 240, 0.8);
            color: #1e293b;
            border-color: #cbd5e1;
        }
        body.light-mode .page-content {
             background-color: rgba(226, 232, 240, 0.4); /* Lighter content background for light mode */
        }
        body.light-mode main#search-page.page-content {
            background-color: transparent;
        }
        body.light-mode .search-input,
        body.light-mode .search-btn,
        body.light-mode .lucky-btn,
        body.light-mode .form-input {
            background-color: #ffffff;
            border-color: #cbd5e1;
            color: #1e293b;
        }
        body.light-mode .text-gray-200 { color: #1e293b; }
        body.light-mode .text-gray-300 { color: #334155; }
        body.light-mode .text-gray-400 { color: #475569; }
        body.light-mode .text-gray-500 { color: #64748b; }
        body.light-mode .hover\:text-white:hover { color: #0f172a; }
        body.light-mode .border-gray-700 { border-color: #cbd5e1; }
        body.light-mode .hover\:bg-gray-600:hover { background-color: #f1f5f9; }
        body.light-mode .settings-tab.active { background-color: #3498db; color: #fff; }
        body.light-mode .settings-tab { color: #334155; }
        body.light-mode .settings-tab:hover { background-color: #f1f5f9; }
        body.light-mode .svg-logo { filter: drop-shadow(0 0 10px rgba(52, 152, 219, 0.8)); }
        body.light-mode .svg-logo:hover { filter: drop-shadow(0 0 15px rgba(52, 152, 219, 1)); }
        body.light-mode .svg-logo g { stroke: #1e293b; }


        /* Toggle Switch */
        .toggle-checkbox:checked {
            right: 0;
            border-color: #3498db;
        }
        .toggle-checkbox:checked + .toggle-label {
            background-color: #3498db;
        }

        /* Settings Page */
        .settings-tab {
            transition: all 0.2s ease;
        }
        .settings-tab.active {
            background-color: #3498db;
            color: #fff;
        }
        .setting-content {
            display: none;
        }
        .setting-content.active {
            display: block;
        }
        
        .bg-thumbnail {
            width: 100%;
            height: 70px;
            background-size: cover;
            background-position: center;
            border-radius: 0.5rem;
            cursor: pointer;
            border: 3px solid transparent;
            transition: border-color 0.2s ease;
        }
        .bg-thumbnail.active {
            border-color: #3498db;
        }

        /* Listening animation */
        .listening .fa-microphone {
            color: #e74c3c;
            animation: pulse 1.5s infinite;
        }
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }
    </style>
</head>
<body class="bg-gray-900 text-gray-200">
    <div id="background-slideshow"></div>
    <div id="background-overlay"></div>

    <!-- Header -->
    <header class="flex justify-between items-center p-4 w-full relative z-10">
        <div class="flex items-center space-x-6">
            <a href="#about" class="nav-link hover:text-white">About</a>
            <a href="#store" class="nav-link hover:text-white">Store</a>
        </div>
        <div class="flex items-center space-x-6">
            <a href="#mail" class="nav-link hover:text-white">proDevMail</a>
            <a href="#images" class="nav-link hover:text-white">Images</a>
            <div id="cart-icon-wrapper" class="relative hidden cursor-pointer">
                <i class="fas fa-shopping-cart text-xl"></i>
                <span id="cart-item-count" class="absolute -top-2 -right-2 bg-blue-600 text-white text-xs rounded-full h-5 w-5 flex items-center justify-center">0</span>
            </div>
            <button id="sign-in-btn" data-target="signin" class="nav-link bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-md">
                Sign in
            </button>
        </div>
    </header>

    <!-- Main Content Area -->
    <div id="app-container" class="flex-grow flex flex-col items-center justify-center relative z-10">

        <!-- Search Page -->
        <main id="search-page" class="page-content px-4 justify-center">
            <div class="search-container w-full max-w-2xl flex flex-col items-center">
                <!-- SVG Logo -->
                <svg id="logo" class="svg-logo mb-8" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
                    <defs>
                        <linearGradient id="logo-gradient" x1="0%" y1="0%" x2="100%" y2="100%">
                            <stop offset="0%" stop-color="#3498db"/>
                            <stop offset="100%" stop-color="#5dade2"/>
                        </linearGradient>
                    </defs>
                    
                    <!-- Magnifying glass handle -->
                    <line x1="65" y1="65" x2="85" y2="85" stroke="url(#logo-gradient)" stroke-width="10" stroke-linecap="round"/>
                    
                    <!-- Magnifying glass lens -->
                    <circle cx="45" cy="45" r="32" stroke="url(#logo-gradient)" stroke-width="8" fill="none"/>
                    
                    <!-- Code brackets inside -->
                    <g fill="none" stroke="#e0e0e0" stroke-width="8" stroke-linecap="round" stroke-linejoin="round">
                        <polyline points="38,30 23,45 38,60" />
                        <polyline points="52,30 67,45 52,60" />
                    </g>
                </svg>

                <form id="search-form" class="w-full" action="https://www.google.com/search" method="GET" target="_blank">
                    <div class="search-bar-wrapper">
                        <div class="search-input relative flex items-center w-full h-12 rounded-full focus-within:shadow-lg bg-gray-800 border border-gray-700">
                            <div class="grid place-items-center h-full w-12 text-gray-400"><i class="fas fa-search"></i></div>
                            <input class="h-full w-full outline-none text-sm text-gray-200 bg-transparent pr-12" type="text" id="search-input-field" name="q" placeholder="Search with proDevSearch or type a URL" autocomplete="off" />
                            <div id="mic-btn" class="grid place-items-center h-full w-12 text-gray-400 hover:text-blue-400 cursor-pointer">
                                <i class="fas fa-microphone"></i>
                            </div>
                        </div>
                        <div class="search-bar-glow"></div>
                    </div>
                    <input type="hidden" name="tbm" id="search-type">
                    <div class="flex justify-center space-x-4 mt-8">
                        <button type="submit" class="search-btn bg-gray-700 hover:bg-gray-600 text-gray-200 font-semibold py-2 px-6 rounded-md">proDevSearch</button>
                        <button type="submit" name="btnI" value="I'm Feeling Lucky" class="lucky-btn bg-gray-700 hover:bg-gray-600 text-gray-200 font-semibold py-2 px-6 rounded-md">I'm Feeling Lucky</button>
                    </div>
                </form>
                <div id="filter-container" class="flex flex-wrap justify-center space-x-8 mt-12 border-b border-gray-700">
                    <button class="filter-btn active py-2 px-1 flex items-center space-x-2" data-type=""><i class="fas fa-search"></i><span>All</span></button>
                    <button class="filter-btn py-2 px-1 flex items-center space-x-2" data-type="isch"><i class="fas fa-image"></i><span>Images</span></button>
                    <button class="filter-btn py-2 px-1 flex items-center space-x-2" data-type="vid"><i class="fas fa-video"></i><span>Videos</span></button>
                    <button class="filter-btn py-2 px-1 flex items-center space-x-2" data-type="nws"><i class="fas fa-newspaper"></i><span>News</span></button>
                    <button class="filter-btn py-2 px-1 flex items-center space-x-2" data-type="shop"><i class="fas fa-shopping-cart"></i><span>Shopping</span></button>
                    <button class="filter-btn py-2 px-1 flex items-center space-x-2" data-type="bks"><i class="fas fa-book"></i><span>Books</span></button>
                </div>
            </div>
        </main>

        <!-- About Page -->
        <div id="about-page" class="page-content hidden p-8 justify-center">
            <div class="max-w-4xl mx-auto text-center">
                <h2 class="text-4xl font-bold text-blue-400 mb-4">About proDevSearch</h2>
                <p class="text-lg text-gray-300">proDevSearch is a powerful, open-source search engine designed for developers, by developers. Our mission is to provide a clean, fast, and privacy-focused search experience, cutting through the noise to deliver the results you need.</p>
                <button class="home-link mt-8 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-md">
                    <i class="fas fa-home mr-2"></i>Back to Search
                </button>
            </div>
        </div>

        <!-- Store Page -->
        <div id="store-page" class="page-content hidden p-4 md:p-8">
             <div class="w-full max-w-7xl mx-auto">
                <div class="text-center mb-10">
                    <h2 class="text-4xl font-bold text-blue-400 mb-4">proDev Store</h2>
                    <p class="text-lg text-gray-300">Get your exclusive proDev gear here. All proceeds support the open-source project.</p>
                </div>
                <div class="flex flex-col lg:flex-row gap-8">
                    <!-- Products Grid -->
                    <div id="product-grid" class="flex-grow grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-3 gap-6">
                        <!-- Product cards will be injected here by JavaScript -->
                    </div>
                    <!-- Shopping Cart Sidebar -->
                    <div class="lg:w-1/3 xl:w-1/4">
                        <div class="bg-gray-800 rounded-lg p-6 sticky top-8">
                            <h3 class="text-2xl font-bold border-b border-gray-700 pb-3 mb-4 flex items-center justify-between">
                                <span>Shopping Cart</span>
                                <i class="fas fa-shopping-cart"></i>
                            </h3>
                            <div id="cart-items-container" class="space-y-4 max-h-96 overflow-y-auto pr-2">
                                <!-- Cart items will be injected here -->
                                <p id="empty-cart-message" class="text-gray-400">Your cart is empty.</p>
                            </div>
                            <div class="border-t border-gray-700 pt-4 mt-4">
                                <div class="flex justify-between items-center font-bold text-lg">
                                    <span>Total:</span>
                                    <span id="cart-total">$0.00</span>
                                </div>
                                <button id="checkout-btn" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-4 disabled:bg-gray-600 disabled:cursor-not-allowed" disabled>
                                    Checkout
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="text-center">
                    <button class="home-link mt-12 bg-gray-700 hover:bg-gray-600 text-white font-semibold py-2 px-4 rounded-md">
                        <i class="fas fa-arrow-left mr-2"></i>Back to Search
                    </button>
                </div>
            </div>
        </div>
        
        <!-- proDevMail Page -->
        <div id="mail-page" class="page-content hidden p-8 justify-center">
            <div class="max-w-4xl mx-auto text-center">
                <h2 class="text-4xl font-bold text-blue-400 mb-4">proDevMail</h2>
                <p class="text-lg text-gray-300">Secure, encrypted, and developer-focused email is coming soon.</p>
                <button data-target="create-mail" class="nav-link mt-8 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-6 rounded-md">
                    Create a proDevMail Account
                </button>
                 <button class="home-link mt-4 bg-transparent text-blue-400 font-semibold py-2 px-4 rounded-md">
                    Back to Search
                </button>
            </div>
        </div>
        
        <!-- Create Mail Page -->
        <div id="create-mail-page" class="page-content hidden p-8 justify-center">
            <div class="w-full max-w-md">
              <form class="bg-gray-800 shadow-md rounded px-8 pt-6 pb-8 mb-4">
                <h2 class="text-3xl text-center font-bold text-blue-400 mb-6">Create your proDevMail</h2>
                <div class="mb-4">
                  <label class="block text-gray-300 text-sm font-bold mb-2" for="mail-username">Choose your email address</label>
                  <div class="flex items-center">
                    <input class="shadow appearance-none border rounded-l w-full py-2 px-3 bg-gray-700 text-gray-200 leading-tight focus:outline-none focus:shadow-outline" id="mail-username" type="text" placeholder="username">
                    <span class="bg-gray-700 border border-l-0 border-gray-600 text-gray-400 p-2 rounded-r">@prodevmail.com</span>
                  </div>
                </div>
                <div class="mb-4">
                  <label class="block text-gray-300 text-sm font-bold mb-2" for="mail-password">Password</label>
                  <input class="shadow appearance-none border rounded w-full py-2 px-3 bg-gray-700 text-gray-200 leading-tight focus:outline-none focus:shadow-outline" id="mail-password" type="password" placeholder="******************">
                </div>
                <div class="mb-6">
                  <label class="block text-gray-300 text-sm font-bold mb-2" for="mail-password-confirm">Confirm Password</label>
                  <input class="shadow appearance-none border rounded w-full py-2 px-3 bg-gray-700 text-gray-200 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="mail-password-confirm" type="password" placeholder="******************">
                </div>
                <div class="flex items-center justify-between">
                  <button class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button">
                    Create Account
                  </button>
                  <a href="#mail" class="nav-link inline-block align-baseline font-bold text-sm text-blue-400 hover:text-blue-500">
                    Back to proDevMail
                  </a>
                </div>
              </form>
            </div>
        </div>

        <!-- Images Page -->
        <div id="images-page" class="page-content hidden p-8 justify-center">
            <div class="max-w-4xl mx-auto text-center">
                <h2 class="text-4xl font-bold text-blue-400 mb-4">Image Search</h2>
                <p class="text-lg text-gray-300">This is the image search portal. Functionality to search for images will be integrated here.</p>
                <button class="home-link mt-8 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-md">
                    <i class="fas fa-home mr-2"></i>Back to Search
                </button>
            </div>
        </div>

        <!-- Sign In Page -->
        <div id="signin-page" class="page-content hidden p-8 justify-center">
            <div class="w-full max-w-xs">
              <form class="bg-gray-800 shadow-md rounded px-8 pt-6 pb-8 mb-4">
                <h2 class="text-2xl text-center font-bold text-blue-400 mb-6">Sign In</h2>
                <div class="mb-4">
                  <label class="block text-gray-300 text-sm font-bold mb-2" for="username">Username</label>
                  <input class="form-input shadow appearance-none border rounded w-full py-2 px-3 bg-gray-700 text-gray-200 leading-tight focus:outline-none focus:shadow-outline" id="username" type="text" placeholder="Username">
                </div>
                <div class="mb-6">
                  <label class="block text-gray-300 text-sm font-bold mb-2" for="password">Password</label>
                  <input class="form-input shadow appearance-none border rounded w-full py-2 px-3 bg-gray-700 text-gray-200 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="password" type="password" placeholder="******************">
                </div>
                <div class="flex items-center justify-between">
                  <button class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button">
                    Sign In
                  </button>
                  <a class="inline-block align-baseline font-bold text-sm text-blue-400 hover:text-blue-500" href="#">
                    Forgot Password?
                  </a>
                </div>
              </form>
              <div class="text-center">
                  <button class="home-link text-blue-400 hover:text-blue-500 text-sm font-bold">
                    <i class="fas fa-arrow-left mr-1"></i> Back to Search
                  </button>
              </div>
            </div>
        </div>

        <!-- Settings Page -->
        <div id="settings-page" class="page-content hidden p-8">
            <div class="max-w-4xl w-full mx-auto">
                <h2 class="text-3xl font-bold text-blue-400 mb-8 text-center">Advanced Settings</h2>
                <div class="flex flex-col md:flex-row gap-8">
                    <!-- Settings Navigation -->
                    <div class="md:w-1/4">
                        <nav id="settings-nav" class="flex flex-col space-y-2">
                            <button data-target="setting-general" class="settings-tab text-left p-3 rounded-md font-semibold active"><i class="fas fa-cog w-6 mr-2"></i>General</button>
                            <button data-target="setting-appearance" class="settings-tab text-left p-3 rounded-md font-semibold"><i class="fas fa-paint-brush w-6 mr-2"></i>Appearance</button>
                            <button data-target="setting-search" class="settings-tab text-left p-3 rounded-md font-semibold"><i class="fas fa-search w-6 mr-2"></i>Search</button>
                            <button data-target="setting-privacy" class="settings-tab text-left p-3 rounded-md font-semibold"><i class="fas fa-user-secret w-6 mr-2"></i>Privacy</button>
                        </nav>
                    </div>
                    <!-- Settings Content -->
                    <div class="flex-grow bg-gray-800 p-6 rounded-lg">
                        <!-- General Settings -->
                        <div id="setting-general" class="setting-content active">
                            <h3 class="text-2xl font-bold mb-6 text-blue-300">General Settings</h3>
                            <div class="space-y-6">
                                <div>
                                     <label for="language-select" class="block text-lg font-semibold mb-2">Language</label>
                                     <p class="text-gray-400 text-sm mb-3">Choose the language for your search results and interface.</p>
                                     <select id="language-select" class="w-full bg-gray-700 border border-gray-600 rounded-md p-2 text-white focus:outline-none focus:ring-2 focus:ring-blue-500">
                                        <option>English</option>
                                        <option>Spanish</option>
                                        <option>French</option>
                                        <option>German</option>
                                        <option>Mandarin</option>
                                     </select>
                                </div>
                                <div>
                                     <label for="region-select" class="block text-lg font-semibold mb-2">Region</label>
                                     <p class="text-gray-400 text-sm mb-3">Set your region for more relevant local results.</p>
                                     <select id="region-select" class="w-full bg-gray-700 border border-gray-600 rounded-md p-2 text-white focus:outline-none focus:ring-2 focus:ring-blue-500">
                                        <option>Canada</option>
                                        <option>United States</option>
                                        <option>United Kingdom</option>
                                        <option>Germany</option>
                                        <option>Australia</option>
                                     </select>
                                </div>
                            </div>
                        </div>
                        <!-- Appearance Settings -->
                        <div id="setting-appearance" class="setting-content">
                            <h3 class="text-2xl font-bold mb-6 text-blue-300">Appearance Settings</h3>
                            <div class="space-y-6">
                                <div class="flex items-center justify-between">
                                    <div>
                                        <h4 class="text-lg font-semibold">Theme</h4>
                                        <p class="text-gray-400 text-sm">Switch between light and dark mode.</p>
                                    </div>
                                    <div class="relative inline-block w-10 mr-2 align-middle select-none">
                                        <input type="checkbox" name="toggle" id="theme-toggle" class="toggle-checkbox absolute block w-6 h-6 rounded-full bg-white border-4 appearance-none cursor-pointer"/>
                                        <label for="theme-toggle" class="toggle-label block overflow-hidden h-6 rounded-full bg-gray-600 cursor-pointer"></label>
                                    </div>
                                </div>
                                <div>
                                     <label for="font-size-select" class="block text-lg font-semibold mb-2">Font Size</label>
                                     <p class="text-gray-400 text-sm mb-3">Adjust the text size across the website.</p>
                                     <select id="font-size-select" class="w-full bg-gray-700 border border-gray-600 rounded-md p-2 text-white focus:outline-none focus:ring-2 focus:ring-blue-500">
                                        <option>Small</option>
                                        <option selected>Medium</option>
                                        <option>Large</option>
                                     </select>
                                </div>
                                <div class="pt-4 border-t border-gray-700">
                                    <h4 class="text-lg font-semibold mb-2">Background Style</h4>
                                    <div class="flex items-center space-x-4 mb-4">
                                        <label><input type="radio" name="bg-style" value="slideshow" checked> Slideshow</label>
                                        <label><input type="radio" name="bg-style" value="image"> Static Image</label>
                                        <label><input type="radio" name="bg-style" value="solid"> Solid Color</label>
                                    </div>
                                    <div id="background-image-selector" class="space-y-2">
                                        <p class="text-gray-400 text-sm">Select a static background image.</p>
                                        <div id="background-thumbnail-grid" class="grid grid-cols-3 sm:grid-cols-4 md:grid-cols-6 gap-2">
                                            <!-- Thumbnails will be injected here -->
                                        </div>
                                    </div>
                                    <div id="background-color-selector" class="hidden space-y-2">
                                        <p class="text-gray-400 text-sm">Select a solid background color.</p>
                                        <input type="color" id="background-color-picker" class="w-full h-10 p-1 bg-gray-700 rounded-md cursor-pointer" value="#121212">
                                    </div>
                                </div>
                            </div>
                        </div>
                        <!-- Search Settings -->
                        <div id="setting-search" class="setting-content">
                            <h3 class="text-2xl font-bold mb-6 text-blue-300">Search Settings</h3>
                             <div class="space-y-6">
                                <div class="flex items-center justify-between">
                                    <div>
                                        <h4 class="text-lg font-semibold">Safe Search</h4>
                                        <p class="text-gray-400 text-sm">Filter explicit results from your searches.</p>
                                    </div>
                                     <div class="relative inline-block w-10 mr-2 align-middle select-none">
                                        <input type="checkbox" name="toggle" id="safe-search-toggle" class="toggle-checkbox absolute block w-6 h-6 rounded-full bg-white border-4 appearance-none cursor-pointer" checked/>
                                        <label for="safe-search-toggle" class="toggle-label block overflow-hidden h-6 rounded-full bg-gray-600 cursor-pointer"></label>
                                    </div>
                                </div>
                                <div class="flex items-center justify-between">
                                    <div>
                                        <h4 class="text-lg font-semibold">Open Links in New Tab</h4>
                                        <p class="text-gray-400 text-sm">Automatically open search results in a new browser tab.</p>
                                    </div>
                                     <div class="relative inline-block w-10 mr-2 align-middle select-none">
                                        <input type="checkbox" name="toggle" id="new-tab-toggle" class="toggle-checkbox absolute block w-6 h-6 rounded-full bg-white border-4 appearance-none cursor-pointer" checked/>
                                        <label for="new-tab-toggle" class="toggle-label block overflow-hidden h-6 rounded-full bg-gray-600 cursor-pointer"></label>
                                    </div>
                                </div>
                                <div>
                                     <label for="results-per-page-select" class="block text-lg font-semibold mb-2">Results Per Page</label>
                                     <p class="text-gray-400 text-sm mb-3">Choose how many search results to show on each page.</p>
                                     <select id="results-per-page-select" class="w-full bg-gray-700 border border-gray-600 rounded-md p-2 text-white focus:outline-none focus:ring-2 focus:ring-blue-500">
                                        <option>10</option>
                                        <option selected>25</option>
                                        <option>50</option>
                                     </select>
                                </div>
                            </div>
                        </div>
                        <!-- Privacy Settings -->
                        <div id="setting-privacy" class="setting-content">
                            <h3 class="text-2xl font-bold mb-6 text-blue-300">Privacy Settings</h3>
                             <div class="space-y-6">
                                <div class="flex items-center justify-between">
                                    <div>
                                        <h4 class="text-lg font-semibold">Save Search History</h4>
                                        <p class="text-gray-400 text-sm">Allow proDevSearch to save your search history.</p>
                                    </div>
                                     <div class="relative inline-block w-10 mr-2 align-middle select-none">
                                        <input type="checkbox" name="toggle" id="history-toggle" class="toggle-checkbox absolute block w-6 h-6 rounded-full bg-white border-4 appearance-none cursor-pointer" checked/>
                                        <label for="history-toggle" class="toggle-label block overflow-hidden h-6 rounded-full bg-gray-600 cursor-pointer"></label>
                                    </div>
                                </div>
                                <div class="flex items-center justify-between">
                                    <div>
                                        <h4 class="text-lg font-semibold">Personalized Results</h4>
                                        <p class="text-gray-400 text-sm">Allow personalized results based on your activity.</p>
                                    </div>
                                     <div class="relative inline-block w-10 mr-2 align-middle select-none">
                                        <input type="checkbox" name="toggle" id="personalization-toggle" class="toggle-checkbox absolute block w-6 h-6 rounded-full bg-white border-4 appearance-none cursor-pointer"/>
                                        <label for="personalization-toggle" class="toggle-label block overflow-hidden h-6 rounded-full bg-gray-600 cursor-pointer"></label>
                                    </div>
                                </div>
                                <button class="text-sm text-blue-400 hover:underline">Manage Search History</button>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="text-center mt-8">
                     <button class="home-link bg-gray-700 hover:bg-gray-600 text-white font-semibold py-2 px-4 rounded-md">
                        <i class="fas fa-arrow-left mr-2"></i>Back to Search
                    </button>
                </div>
            </div>
        </div>

        <!-- Checkout Page -->
        <div id="checkout-page" class="page-content hidden p-4 md:p-8">
            <div class="w-full max-w-6xl mx-auto">
                <h2 class="text-4xl font-bold text-blue-400 mb-8 text-center">Checkout</h2>
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-12">
                    <!-- Shipping & Payment Form -->
                    <div class="bg-gray-800 rounded-lg p-6">
                        <form id="checkout-form">
                            <h3 class="text-2xl font-bold mb-6 border-b border-gray-700 pb-3">Shipping & Payment</h3>
                            <!-- Shipping -->
                            <div class="mb-6">
                                <h4 class="text-xl font-semibold mb-4 text-blue-300">Shipping Information</h4>
                                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                                    <div>
                                        <label for="name" class="block text-sm font-medium text-gray-400 mb-1">Full Name</label>
                                        <input type="text" id="name" class="form-input w-full bg-gray-700 border-gray-600 rounded-md p-2" required>
                                    </div>
                                    <div>
                                        <label for="email" class="block text-sm font-medium text-gray-400 mb-1">Email Address</label>
                                        <input type="email" id="email" class="form-input w-full bg-gray-700 border-gray-600 rounded-md p-2" required>
                                    </div>
                                    <div class="sm:col-span-2">
                                        <label for="address" class="block text-sm font-medium text-gray-400 mb-1">Address</label>
                                        <input type="text" id="address" class="form-input w-full bg-gray-700 border-gray-600 rounded-md p-2" required>
                                    </div>
                                    <div>
                                        <label for="city" class="block text-sm font-medium text-gray-400 mb-1">City</label>
                                        <input type="text" id="city" class="form-input w-full bg-gray-700 border-gray-600 rounded-md p-2" required>
                                    </div>
                                    <div>
                                        <label for="postal" class="block text-sm font-medium text-gray-400 mb-1">Postal Code</label>
                                        <input type="text" id="postal" class="form-input w-full bg-gray-700 border-gray-600 rounded-md p-2" required>
                                    </div>
                                </div>
                            </div>
                            <!-- Payment -->
                            <div>
                                <h4 class="text-xl font-semibold mb-4 text-blue-300">Payment Details</h4>
                                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                                    <div class="sm:col-span-2">
                                        <label for="card-number" class="block text-sm font-medium text-gray-400 mb-1">Card Number</label>
                                        <input type="text" id="card-number" placeholder="**** **** **** ****" class="form-input w-full bg-gray-700 border-gray-600 rounded-md p-2" required>
                                    </div>
                                    <div>
                                        <label for="expiry" class="block text-sm font-medium text-gray-400 mb-1">Expiry Date</label>
                                        <input type="text" id="expiry" placeholder="MM/YY" class="form-input w-full bg-gray-700 border-gray-600 rounded-md p-2" required>
                                    </div>
                                    <div>
                                        <label for="cvc" class="block text-sm font-medium text-gray-400 mb-1">CVC</label>
                                        <input type="text" id="cvc" placeholder="***" class="form-input w-full bg-gray-700 border-gray-600 rounded-md p-2" required>
                                    </div>
                                </div>
                            </div>
                        </form>
                    </div>
                    <!-- Order Summary -->
                    <div class="bg-gray-800 rounded-lg p-6">
                        <h3 class="text-2xl font-bold mb-6 border-b border-gray-700 pb-3">Order Summary</h3>
                        <div id="checkout-summary-container" class="space-y-4">
                            <!-- Checkout summary items will be injected here -->
                        </div>
                        <div class="border-t border-gray-700 pt-4 mt-6">
                            <div class="flex justify-between items-center text-lg mb-4">
                                <span>Subtotal</span>
                                <span id="checkout-subtotal">$0.00</span>
                            </div>
                            <div class="flex justify-between items-center text-lg mb-4">
                                <span>Shipping</span>
                                <span>$5.00</span>
                            </div>
                            <div class="flex justify-between items-center font-bold text-xl">
                                <span>Total</span>
                                <span id="checkout-total">$0.00</span>
                            </div>
                        </div>
                        <button id="place-order-btn" type="submit" form="checkout-form" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-4 rounded mt-6">
                            Place Order
                        </button>
                    </div>
                </div>
                 <div class="text-center">
                     <button data-target="store" class="nav-link mt-12 bg-gray-700 hover:bg-gray-600 text-white font-semibold py-2 px-4 rounded-md">
                        <i class="fas fa-arrow-left mr-2"></i>Back to Store
                    </button>
                </div>
            </div>
        </div>

        <!-- Order Confirmation Page -->
        <div id="order-confirmation-page" class="page-content hidden p-8 justify-center">
            <div class="max-w-2xl mx-auto text-center bg-gray-800 p-10 rounded-lg">
                <i class="fas fa-check-circle text-6xl text-green-500 mb-6"></i>
                <h2 class="text-4xl font-bold text-blue-400 mb-4">Thank You For Your Order!</h2>
                <p class="text-lg text-gray-300 mb-2">Your order has been placed successfully.</p>
                <p class="text-gray-400">Your order number is <strong id="order-number" class="text-white">#000000</strong>.</p>
                <button data-target="store" class="nav-link mt-8 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-6 rounded-md">
                    Continue Shopping
                </button>
            </div>
        </div>

        <!-- Other Pages -->
        <div id="advertising-page" class="page-content hidden p-8 justify-center">...</div>
        <div id="business-page" class="page-content hidden p-8 justify-center">...</div>
        <div id="privacy-page" class="page-content hidden p-8 justify-center">...</div>
        <div id="terms-page" class="page-content hidden p-8 justify-center">...</div>
        <div id="how-search-works-page" class="page-content hidden p-8">...</div>

    </div>

    <!-- Footer -->
    <footer class="bg-gray-800 text-gray-500 text-sm w-full relative z-10">
        <div class="px-8 py-4">
            <p>Canada</p>
        </div>
        <div class="flex flex-wrap justify-center md:justify-between items-center px-8 py-4 border-t border-gray-700">
            <div class="flex space-x-6 mb-4 md:mb-0">
                <a href="#advertising" class="nav-link hover:text-gray-300">Advertising</a>
                <a href="#business" class="nav-link hover:text-gray-300">Business</a>
                <a href="#how-search-works" class="nav-link hover:text-gray-300">How Search works</a>
                <a href="#about" class="nav-link hover:text-gray-300">Open Source</a>
            </div>
            <div class="flex space-x-6">
                <a href="#privacy" class="nav-link hover:text-gray-300">Privacy</a>
                <a href="#terms" class="nav-link hover:text-gray-300">Terms</a>
                <a href="#settings" class="nav-link hover:text-gray-300">Settings</a>
            </div>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
             // --- Background Slideshow & Settings ---
            const slideshow = document.getElementById('background-slideshow');
            const backgroundOptions = [
                { thumb: 'https://images.unsplash.com/photo-1500239038240-9b3b818e736a?q=80&w=200&auto=format&fit=crop', full: 'https://images.unsplash.com/photo-1500239038240-9b3b818e736a?q=80&w=2940&auto=format&fit=crop' },
                { thumb: 'https://images.unsplash.com/photo-1433086966358-54859d0ed716?q=80&w=200&auto=format&fit=crop', full: 'https://images.unsplash.com/photo-1433086966358-54859d0ed716?q=80&w=2940&auto=format&fit=crop' },
                { thumb: 'https://images.unsplash.com/photo-1476231682828-37e571bc172f?q=80&w=200&auto=format&fit=crop', full: 'https://images.unsplash.com/photo-1476231682828-37e571bc172f?q=80&w=2874&auto=format&fit=crop' },
                { thumb: 'https://images.unsplash.com/photo-1447752875215-b2761acb3c5d?q=80&w=200&auto=format&fit=crop', full: 'https://images.unsplash.com/photo-1447752875215-b2761acb3c5d?q=80&w=2940&auto=format&fit=crop' },
                { thumb: 'https://images.unsplash.com/photo-1532274402911-5a369e4c4bb5?q=80&w=200&auto=format&fit=crop', full: 'https://images.unsplash.com/photo-1532274402911-5a369e4c4bb5?q=80&w=2940&auto=format&fit=crop' },
                { thumb: 'https://images.unsplash.com/photo-1470770841072-f978cf4d019e?q=80&w=200&auto=format&fit=crop', full: 'https://images.unsplash.com/photo-1470770841072-f978cf4d019e?q=80&w=2940&auto=format&fit=crop' },
            ];
            let currentBgIndex = 0;
            let slideshowInterval;

            const bgStyleRadios = document.querySelectorAll('input[name="bg-style"]');
            const imageSelector = document.getElementById('background-image-selector');
            const colorSelector = document.getElementById('background-color-selector');
            const thumbnailGrid = document.getElementById('background-thumbnail-grid');
            const colorPicker = document.getElementById('background-color-picker');

            function setBackgroundImage(url) {
                if(slideshow) slideshow.style.backgroundImage = `url('${url}')`;
                document.body.style.backgroundColor = ''; // Clear solid color
            }
            
            function setSolidColor(color) {
                if(slideshow) slideshow.style.backgroundImage = 'none';
                document.body.style.backgroundColor = color;
            }

            function startSlideshow() {
                stopSlideshow(); // Ensure no multiple intervals
                slideshowInterval = setInterval(() => {
                    currentBgIndex = (currentBgIndex + 1) % backgroundOptions.length;
                    setBackgroundImage(backgroundOptions[currentBgIndex].full);
                }, 7000);
            }

            function stopSlideshow() {
                clearInterval(slideshowInterval);
            }

            function populateThumbnails() {
                if(!thumbnailGrid) return;
                thumbnailGrid.innerHTML = '';
                backgroundOptions.forEach((option, index) => {
                    const thumbDiv = document.createElement('div');
                    thumbDiv.className = 'bg-thumbnail';
                    thumbDiv.style.backgroundImage = `url(${option.thumb})`;
                    thumbDiv.dataset.index = index;
                    thumbnailGrid.appendChild(thumbDiv);
                });
            }

            if (thumbnailGrid) {
                thumbnailGrid.addEventListener('click', (e) => {
                    if (e.target.classList.contains('bg-thumbnail')) {
                        const index = e.target.dataset.index;
                        currentBgIndex = parseInt(index);
                        setBackgroundImage(backgroundOptions[currentBgIndex].full);
                        
                        // Update active class
                        document.querySelectorAll('.bg-thumbnail').forEach(t => t.classList.remove('active'));
                        e.target.classList.add('active');
                    }
                });
            }

            if (bgStyleRadios.length > 0) {
                bgStyleRadios.forEach(radio => {
                    radio.addEventListener('change', (e) => {
                        const mode = e.target.value;
                        if (mode === 'slideshow') {
                            imageSelector.style.display = 'none';
                            colorSelector.style.display = 'none';
                            setBackgroundImage(backgroundOptions[currentBgIndex].full);
                            startSlideshow();
                        } else if (mode === 'image') {
                            stopSlideshow();
                            imageSelector.style.display = 'block';
                            colorSelector.style.display = 'none';
                            // Set to current image if one isn't active
                             if (!document.querySelector('.bg-thumbnail.active')) {
                                document.querySelector('.bg-thumbnail').classList.add('active');
                            }
                            setBackgroundImage(backgroundOptions[currentBgIndex].full);
                        } else if (mode === 'solid') {
                            stopSlideshow();
                            imageSelector.style.display = 'none';
                            colorSelector.style.display = 'block';
                            setSolidColor(colorPicker.value);
                        }
                    });
                });
            }
            
            if(colorPicker) {
                colorPicker.addEventListener('input', (e) => {
                    setSolidColor(e.target.value);
                });
            }

            // Initial setup
            populateThumbnails();
            setBackgroundImage(backgroundOptions[currentBgIndex].full);
            startSlideshow();
            if(imageSelector) imageSelector.style.display = 'none';
            if(colorSelector) colorSelector.style.display = 'none';


            // --- Settings Logic ---
            const themeToggle = document.getElementById('theme-toggle');
            if(themeToggle) {
                themeToggle.addEventListener('change', () => {
                    document.body.classList.toggle('light-mode');
                });
            }
            
            const settingsNav = document.getElementById('settings-nav');
            const settingsTabs = document.querySelectorAll('.settings-tab');
            const settingContents = document.querySelectorAll('.setting-content');

            if (settingsNav) {
                settingsNav.addEventListener('click', (e) => {
                    const targetButton = e.target.closest('.settings-tab');
                    if (!targetButton) return;

                    const targetContentId = targetButton.dataset.target;

                    settingsTabs.forEach(tab => tab.classList.remove('active'));
                    targetButton.classList.add('active');

                    settingContents.forEach(content => {
                        if (content.id === targetContentId) {
                            content.classList.add('active');
                        } else {
                            content.classList.remove('active');
                        }
                    });
                });
            }


            // --- Store Data and Logic ---
            const products = [
                { id: 1, name: 'proDev Hoodie', price: 49.99, imageUrl: 'https://placehold.co/400x400/334155/e2e8f0?text=proDevSearch%0A%0AHoodie', sizes: ['S', 'M', 'L', 'XL', 'XXL'], colors: [{ name: 'Charcoal', hex: '#334155' }, { name: 'Navy', hex: '#1e3a8a' }, { name: 'Maroon', hex: '#881337' }] },
                { id: 2, name: 'proDev T-Shirt', price: 24.99, imageUrl: 'https://placehold.co/400x400/1e293b/e2e8f0?text=proDevSearch%0A%0AT-Shirt', sizes: ['S', 'M', 'L', 'XL'], colors: [{ name: 'Black', hex: '#1e293b' }, { name: 'White', hex: '#ffffff' }, { name: 'Heather Grey', hex: '#d1d5db' }] },
                { id: 3, name: 'proDev Sticker Pack', price: 9.99, imageUrl: 'https://placehold.co/400x400/4f46e5/e2e8f0?text=proDevSearch%0A%0AStickers', sizes: ['One Size'], colors: [{ name: 'Default', hex: '#ffffff'}] },
                { id: 4, name: 'proDev Mug', price: 14.99, imageUrl: 'https://placehold.co/400x400/f1f5f9/1e293b?text=proDevSearch%0A%0AMug', sizes: ['11oz'], colors: [{ name: 'White', hex: '#ffffff'}] },
            ];

            let cart = [];

            const productGrid = document.getElementById('product-grid');
            const cartItemsContainer = document.getElementById('cart-items-container');
            const cartTotalEl = document.getElementById('cart-total');
            const emptyCartMessage = document.getElementById('empty-cart-message');
            const checkoutBtn = document.getElementById('checkout-btn');
            const cartIconWrapper = document.getElementById('cart-icon-wrapper');
            const cartItemCountEl = document.getElementById('cart-item-count');
            
            // Checkout Page Elements
            const checkoutForm = document.getElementById('checkout-form');
            const placeOrderBtn = document.getElementById('place-order-btn');
            const checkoutSummaryContainer = document.getElementById('checkout-summary-container');
            const checkoutSubtotalEl = document.getElementById('checkout-subtotal');
            const checkoutTotalEl = document.getElementById('checkout-total');
            const orderNumberEl = document.getElementById('order-number');

            function renderProducts() {
                if (!productGrid) return;
                productGrid.innerHTML = '';
                products.forEach(product => {
                    const sizeOptions = product.sizes.map(size => `<option value="${size}">${size}</option>`).join('');
                    const colorOptions = product.colors.map(color => `<option value="${color.name}">${color.name}</option>`).join('');
                    const productCard = `<div class="product-card bg-gray-800 rounded-lg p-4 flex flex-col" data-product-id="${product.id}"><img src="${product.imageUrl}" alt="${product.name}" class="rounded-md mb-4 h-60 w-full object-cover" onerror="this.onerror=null;this.src='https://placehold.co/400x400/ef4444/ffffff?text=Image+Not+Found';"><h3 class="text-xl font-bold text-white flex-grow">${product.name}</h3><div class="grid grid-cols-2 gap-4 mt-4"><div><label for="size-select-${product.id}" class="text-sm font-medium text-gray-400">Size</label><select id="size-select-${product.id}" class="size-select mt-1 w-full bg-gray-700 border border-gray-600 rounded-md p-2 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">${sizeOptions}</select></div><div><label for="color-select-${product.id}" class="text-sm font-medium text-gray-400">Color</label><select id="color-select-${product.id}" class="color-select mt-1 w-full bg-gray-700 border border-gray-600 rounded-md p-2 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">${colorOptions}</select></div></div><div class="flex justify-between items-center mt-6"><p class="text-lg font-semibold text-blue-400">$${product.price.toFixed(2)}</p><button data-product-id="${product.id}" class="add-to-cart-btn bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-md">Add to Cart</button></div></div>`;
                    productGrid.innerHTML += productCard;
                });
            }

            function renderCart() {
                if (!cartItemsContainer) return;
                cartItemsContainer.innerHTML = '';
                if (cart.length === 0) {
                    if(emptyCartMessage) {
                        emptyCartMessage.classList.remove('hidden');
                        cartItemsContainer.appendChild(emptyCartMessage);
                    }
                } else {
                    if(emptyCartMessage) emptyCartMessage.classList.add('hidden');
                    cart.forEach(item => {
                        const cartItem = `<div class="flex justify-between items-center text-sm"><div class="flex items-center gap-3"><img src="${item.imageUrl}" class="w-12 h-12 rounded object-cover" onerror="this.onerror=null;this.src='https://placehold.co/40x40/ef4444/ffffff?text=N/A';"><div class="flex-grow"><p class="font-bold">${item.name}</p><p class="text-gray-400 text-xs">${item.size} / ${item.color}</p><p class="text-gray-300 font-semibold">$${item.price.toFixed(2)}</p></div></div><button data-cart-id="${item.cartId}" class="cart-item-remove-btn text-gray-500 p-2"><i class="fas fa-trash-alt"></i></button></div>`;
                        cartItemsContainer.innerHTML += cartItem;
                    });
                }
                const total = cart.reduce((sum, item) => sum + item.price, 0);
                if (cartTotalEl) cartTotalEl.textContent = `$${total.toFixed(2)}`;
                if (checkoutBtn) checkoutBtn.disabled = cart.length === 0;
                if (cartItemCountEl) cartItemCountEl.textContent = cart.length;
                if (cartIconWrapper) cartIconWrapper.classList.toggle('hidden', cart.length === 0);
            }

            function renderCheckoutSummary() {
                if (!checkoutSummaryContainer) return;
                checkoutSummaryContainer.innerHTML = '';
                cart.forEach(item => {
                    const summaryItem = `<div class="flex justify-between items-center text-sm"><div class="flex items-center gap-3"><img src="${item.imageUrl}" class="w-16 h-16 rounded object-cover"><div class="flex-grow"><p class="font-bold">${item.name}</p><p class="text-gray-400 text-xs">${item.size} / ${item.color}</p></div></div><p class="font-semibold">$${item.price.toFixed(2)}</p></div>`;
                    checkoutSummaryContainer.innerHTML += summaryItem;
                });
                const subtotal = cart.reduce((sum, item) => sum + item.price, 0);
                const shipping = 5.00;
                const total = subtotal + shipping;
                if(checkoutSubtotalEl) checkoutSubtotalEl.textContent = `$${subtotal.toFixed(2)}`;
                if(checkoutTotalEl) checkoutTotalEl.textContent = `$${total.toFixed(2)}`;
            }

            if(productGrid) {
                productGrid.addEventListener('click', e => {
                    if (e.target.classList.contains('add-to-cart-btn')) {
                        const card = e.target.closest('.product-card');
                        const productId = parseInt(card.dataset.productId);
                        const productToAdd = products.find(p => p.id === productId);
                        const selectedSize = card.querySelector('.size-select').value;
                        const selectedColor = card.querySelector('.color-select').value;
                        cart.push({ ...productToAdd, size: selectedSize, color: selectedColor, cartId: Date.now() + Math.random() });
                        renderCart();
                    }
                });
            }

            if(cartItemsContainer) {
                cartItemsContainer.addEventListener('click', e => {
                    const removeButton = e.target.closest('.cart-item-remove-btn');
                    if (removeButton) {
                        const cartIdToRemove = parseFloat(removeButton.dataset.cartId);
                        cart = cart.filter(item => item.cartId !== cartIdToRemove);
                        renderCart();
                    }
                });
            }
            
            if(checkoutBtn) {
                checkoutBtn.addEventListener('click', () => {
                    showPage('checkout');
                });
            }

            if(checkoutForm) {
                checkoutForm.addEventListener('submit', (e) => {
                    e.preventDefault();
                    if (orderNumberEl) orderNumberEl.textContent = `#${Math.floor(Math.random() * 900000) + 100000}`;
                    cart = [];
                    renderCart();
                    showPage('order-confirmation');
                });
            }

            // --- Page Navigation Logic ---
            const pages = document.querySelectorAll('.page-content');
            const navLinks = document.querySelectorAll('.nav-link');
            const homeLinks = document.querySelectorAll('.home-link');
            const logo = document.getElementById('logo');

            function showPage(pageId) {
                if (!pageId) return;
                pages.forEach(page => {
                    page.classList.add('hidden');
                });
                const pageToShow = document.getElementById(pageId + '-page');
                if(pageToShow) pageToShow.classList.remove('hidden');

                if (pageId === 'store') {
                    renderProducts();
                    renderCart();
                }
                if (pageId === 'checkout') {
                    renderCheckoutSummary();
                }
                if (cartIconWrapper) cartIconWrapper.classList.toggle('hidden', pageId !== 'store');
            }

            if(cartIconWrapper) {
                cartIconWrapper.addEventListener('click', () => {
                    showPage('store');
                });
            }

            navLinks.forEach(link => {
                link.addEventListener('click', (e) => {
                    e.preventDefault();
                    let targetPage = e.currentTarget.dataset.target || new URL(e.currentTarget.href).hash.substring(1);
                    if(targetPage) showPage(targetPage);
                });
            });

            homeLinks.forEach(link => {
                link.addEventListener('click', (e) => {
                    e.preventDefault();
                    showPage('search');
                });
            });
            
            if(logo) logo.addEventListener('click', () => showPage('search'));

            // --- Speech Recognition Logic ---
            const micBtn = document.getElementById('mic-btn');
            const searchInputEl = document.getElementById('search-input-field');

            const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
            if (SpeechRecognition && micBtn) {
                const recognition = new SpeechRecognition();
                recognition.continuous = false;
                recognition.lang = 'en-US';
                recognition.interimResults = false;

                micBtn.addEventListener('click', () => {
                    recognition.start();
                });

                recognition.onstart = () => {
                    micBtn.classList.add('listening');
                };

                recognition.onresult = (event) => {
                    const transcript = event.results[event.results.length - 1][0].transcript.trim();
                    searchInputEl.value = transcript;
                };

                recognition.onerror = (event) => {
                    console.error('Speech recognition error:', event.error);
                };

                recognition.onend = () => {
                    micBtn.classList.remove('listening');
                };

            } else if (micBtn) {
                micBtn.style.display = 'none';
            }

            // --- Original Search Logic ---
            const searchForm = document.getElementById('search-form');
            const searchTypeInput = document.getElementById('search-type');
            const filterButtons = document.querySelectorAll('.filter-btn');

            if(filterButtons) {
                filterButtons.forEach(button => {
                    button.addEventListener('click', (e) => {
                        filterButtons.forEach(btn => btn.classList.remove('active'));
                        e.currentTarget.classList.add('active');
                        if (searchTypeInput) searchTypeInput.value = e.currentTarget.dataset.type;
                        if (searchInputEl) searchInputEl.focus();
                    });
                });
            }
            
            // Initial render
            if(document.getElementById('store-page') && !document.getElementById('store-page').classList.contains('hidden')) {
                 renderProducts();
                 renderCart();
            }
        });
    </script>

</body>
</html>
