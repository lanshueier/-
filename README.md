!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>分屏助手 - 一键双应用分屏与快捷切换</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.7.2/css/all.min.css" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#165DFF',
                        secondary: '#36D399',
                        accent: '#FF9F43',
                        dark: '#1E293B',
                        light: '#F8FAFC'
                    },
                    fontFamily: {
                        inter: ['Inter', 'system-ui', 'sans-serif'],
                    },
                }
            }
        }
    </script>
    <style type="text/tailwindcss">
        @layer utilities {
            .content-auto {
                content-visibility: auto;
            }
            .glass {
                backdrop-filter: blur(12px);
                -webkit-backdrop-filter: blur(12px);
            }
            .shadow-custom {
                box-shadow: 0 8px 30px rgba(0, 0, 0, 0.12);
            }
            .app-card-hover {
                transition: transform 0.3s ease, box-shadow 0.3s ease;
            }
            .app-card-hover:hover {
                transform: translateY(-4px);
                box-shadow: 0 12px 40px rgba(22, 93, 255, 0.18);
            }
            .float-button {
                transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275), 
                            box-shadow 0.3s ease;
            }
            .float-button:hover {
                transform: scale(1.08);
                box-shadow: 0 10px 25px rgba(22, 93, 255, 0.3);
            }
            .float-button:active {
                transform: scale(0.95);
            }
            .ripple {
                position: absolute;
                border-radius: 50%;
                transform: scale(0);
                animation: ripple 0.6s linear;
                background-color: rgba(255, 255, 255, 0.3);
            }
            @keyframes ripple {
                to {
                    transform: scale(2.5);
                    opacity: 0;
                }
            }
            .app-grid {
                display: grid;
                grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
                gap: 16px;
            }
            @media (max-width: 640px) {
                .app-grid {
                    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
                    gap: 12px;
                }
            }
            .animate-fade-in {
                animation: fadeIn 0.4s ease forwards;
            }
            @keyframes fadeIn {
                from { opacity: 0; transform: translateY(10px); }
                to { opacity: 1; transform: translateY(0); }
            }
            .animate-slide-in {
                animation: slideIn 0.3s ease forwards;
            }
            @keyframes slideIn {
                from { transform: translateX(-100%); }
                to { transform: translateX(0); }
            }
            .animate-pulse-light {
                animation: pulseLight 2s infinite;
            }
            @keyframes pulseLight {
                0%, 100% { opacity: 1; }
                50% { opacity: 0.7; }
            }
        }
    </style>
</head>
<body class="font-inter bg-gray-50 text-gray-800 min-h-screen flex flex-col">
    <!-- 顶部导航栏 -->
    <header class="bg-white shadow-sm sticky top-0 z-50">
        <div class="container mx-auto px-4 py-3 flex items-center justify-between">
            <div class="flex items-center space-x-2">
                <div class="bg-primary text-white p-2 rounded-lg">
                    <i class="fa-solid fa-columns text-xl"></i>
                </div>
                <h1 class="text-xl font-bold text-dark">分屏助手</h1>
            </div>
            <div class="flex items-center space-x-4">
                <button class="text-gray-600 hover:text-primary transition-colors">
                    <i class="fa-solid fa-question-circle text-lg"></i>
                </button>
                <button class="text-gray-600 hover:text-primary transition-colors">
                    <i class="fa-solid fa-cog text-lg"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- 主内容区 -->
    <main class="flex-grow container mx-auto px-4 py-6">
        <!-- 功能介绍卡片 -->
        <section class="mb-8 animate-fade-in">
            <div class="bg-gradient-to-r from-primary/90 to-primary rounded-2xl p-6 shadow-lg text-white">
                <h2 class="text-[clamp(1.5rem,3vw,2rem)] font-bold mb-3">一键双应用分屏</h2>
                <p class="mb-4 opacity-90">轻松同时运行两个应用，提高工作效率。支持任意应用组合，自定义分屏比例。</p>
                <div class="flex flex-wrap gap-3">
                    <button class="bg-white text-primary px-5 py-2 rounded-full font-medium hover:bg-gray-100 transition-colors shadow-md">
                        <i class="fa-solid fa-plus mr-2"></i>创建分屏组合
                    </button>
                    <button class="bg-white/20 text-white px-5 py-2 rounded-full font-medium hover:bg-white/30 transition-colors">
                        <i class="fa-solid fa-lightbulb mr-2"></i>查看教程
                    </button>
                </div>
            </div>
        </section>

        <!-- 最近使用的应用 -->
        <section class="mb-8 animate-fade-in" style="animation-delay: 0.1s">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-xl font-bold text-dark">最近使用</h2>
                <button class="text-primary hover:text-primary/80 transition-colors text-sm">
                    查看全部
                </button>
            </div>
            <div class="app-grid">
                <!-- 应用卡片 -->
                <div class="app-card-hover bg-white rounded-xl p-3 shadow-custom text-center cursor-pointer">
                    <div class="w-14 h-14 mx-auto mb-2 rounded-lg bg-[#FF5252] flex items-center justify-center">
                        <i class="fa-brands fa-whatsapp text-white text-2xl"></i>
                    </div>
                    <p class="text-sm font-medium">WhatsApp</p>
                </div>
                
                <div class="app-card-hover bg-white rounded-xl p-3 shadow-custom text-center cursor-pointer">
                    <div class="w-14 h-14 mx-auto mb-2 rounded-lg bg-[#25D366] flex items-center justify-center">
                        <i class="fa-brands fa-instagram text-white text-2xl"></i>
                    </div>
                    <p class="text-sm font-medium">Instagram</p>
                </div>
                
                <div class="app-card-hover bg-white rounded-xl p-3 shadow-custom text-center cursor-pointer">
                    <div class="w-14 h-14 mx-auto mb-2 rounded-lg bg-[#4285F4] flex items-center justify-center">
                        <i class="fa-brands fa-chrome text-white text-2xl"></i>
                    </div>
                    <p class="text-sm font-medium">Chrome</p>
                </div>
                
                <div class="app-card-hover bg-white rounded-xl p-3 shadow-custom text-center cursor-pointer">
                    <div class="w-14 h-14 mx-auto mb-2 rounded-lg bg-[#1DA1F2] flex items-center justify-center">
                        <i class="fa-brands fa-twitter text-white text-2xl"></i>
                    </div>
                    <p class="text-sm font-medium">Twitter</p>
                </div>
                
                <div class="app-card-hover bg-white rounded-xl p-3 shadow-custom text-center cursor-pointer">
                    <div class="w-14 h-14 mx-auto mb-2 rounded-lg bg-[#1877F2] flex items-center justify-center">
                        <i class="fa-brands fa-facebook text-white text-2xl"></i>
                    </div>
                    <p class="text-sm font-medium">Facebook</p>
                </div>
                
                <div class="app-card-hover bg-white rounded-xl p-3 shadow-custom text-center cursor-pointer">
                    <div class="w-14 h-14 mx-auto mb-2 rounded-lg bg-[#FF4500] flex items-center justify-center">
                        <i class="fa-brands fa-reddit text-white text-2xl"></i>
                    </div>
                    <p class="text-sm font-medium">Reddit</p>
                </div>
            </div>
        </section>

        <!-- 常用分屏组合 -->
        <section class="mb-8 animate-fade-in" style="animation-delay: 0.2s">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-xl font-bold text-dark">常用分屏组合</h2>
                <button class="text-primary hover:text-primary/80 transition-colors text-sm">
                    管理组合
                </button>
            </div>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-5">
                <!-- 分屏组合卡片 -->
                <div class="app-card-hover bg-white rounded-xl shadow-custom overflow-hidden">
                    <div class="flex h-32">
                        <div class="w-1/2 bg-[#FF5252] flex items-center justify-center">
                            <i class="fa-brands fa-whatsapp text-white text-3xl"></i>
                        </div>
                        <div class="w-1/2 bg-[#4285F4] flex items-center justify-center">
                            <i class="fa-brands fa-chrome text-white text-3xl"></i>
                        </div>
                    </div>
                    <div class="p-4">
                        <div class="flex justify-between items-center mb-2">
                            <h3 class="font-medium">社交浏览</h3>
                            <span class="text-xs bg-primary/10 text-primary px-2 py-1 rounded-full">
                                最常用
                            </span>
                        </div>
                        <p class="text-sm text-gray-500 mb-3">同时使用 WhatsApp 和 Chrome 浏览器</p>
                        <button class="w-full bg-primary text-white py-2 rounded-lg hover:bg-primary/90 transition-colors float-button">
                            启动分屏
                        </button>
                    </div>
                </div>
                
                <div class="app-card-hover bg-white rounded-xl shadow-custom overflow-hidden">
                    <div class="flex h-32">
                        <div class="w-1/2 bg-[#1877F2] flex items-center justify-center">
                            <i class="fa-brands fa-facebook text-white text-3xl"></i>
                        </div>
                        <div class="w-1/2 bg-[#FF4500] flex items-center justify-center">
                            <i class="fa-brands fa-reddit text-white text-3xl"></i>
                        </div>
                    </div>
                    <div class="p-4">
                        <div class="flex justify-between items-center mb-2">
                            <h3 class="font-medium">双社交</h3>
                            <span class="text-xs bg-gray-100 text-gray-600 px-2 py-1 rounded-full">
                                工作效率
                            </span>
                        </div>
                        <p class="text-sm text-gray-500 mb-3">同时使用 Facebook 和 Reddit</p>
                        <button class="w-full bg-primary text-white py-2 rounded-lg hover:bg-primary/90 transition-colors float-button">
                            启动分屏
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- 所有应用列表 -->
        <section class="animate-fade-in" style="animation-delay: 0.3s">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-xl font-bold text-dark">所有应用</h2>
                <div class="relative">
                    <input type="text" placeholder="搜索应用..." class="pl-9 pr-4 py-2 rounded-lg border border-gray-200 focus:outline-none focus:ring-2 focus:ring-primary/30 focus:border-primary transition-all w-48">
                    <i class="fa-solid fa-search absolute left-3 top-1/2 -translate-y-1/2 text-gray-400"></i>
                </div>
            </div>
            
            <div class="bg-white rounded-xl shadow-custom p-4">
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                    <!-- 应用列表项 -->
                    <div class="app-card-hover flex items-center p-3 rounded-lg cursor-pointer">
                        <div class="w-10 h-10 rounded-lg bg-[#FF5252] flex items-center justify-center mr-3">
                            <i class="fa-brands fa-whatsapp text-white"></i>
                        </div>
                        <div class="flex-grow">
                            <h4 class="font-medium">WhatsApp</h4>
                            <p class="text-xs text-gray-500">通讯社交</p>
                        </div>
                        <button class="text-primary hover:text-primary/80 transition-colors">
                            <i class="fa-solid fa-plus"></i>
                        </button>
                    </div>
                    
                    <div class="app-card-hover flex items-center p-3 rounded-lg cursor-pointer">
                        <div class="w-10 h-10 rounded-lg bg-[#25D366] flex items-center justify-center mr-3">
                            <i class="fa-brands fa-instagram text-white"></i>
                        </div>
                        <div class="flex-grow">
                            <h4 class="font-medium">Instagram</h4>
                            <p class="text-xs text-gray-500">社交媒体</p>
                        </div>
                        <button class="text-primary hover:text-primary/80 transition-colors">
                            <i class="fa-solid fa-plus"></i>
                        </button>
                    </div>
                    
                    <div class="app-card-hover flex items-center p-3 rounded-lg cursor-pointer">
                        <div class="w-10 h-10 rounded-lg bg-[#4285F4] flex items-center justify-center mr-3">
                            <i class="fa-brands fa-chrome text-white"></i>
                        </div>
                        <div class="flex-grow">
                            <h4 class="font-medium">Chrome</h4>
                            <p class="text-xs text-gray-500">网络浏览</p>
                        </div>
                        <button class="text-primary hover:text-primary/80 transition-colors">
                            <i class="fa-solid fa-plus"></i>
                        </button>
                    </div>
                    
                    <div class="app-card-hover flex items-center p-3 rounded-lg cursor-pointer">
                        <div class="w-10 h-10 rounded-lg bg-[#1DA1F2] flex items-center justify-center mr-3">
                            <i class="fa-brands fa-twitter text-white"></i>
                        </div>
                        <div class="flex-grow">
                            <h4 class="font-medium">Twitter</h4>
                            <p class="text-xs text-gray-500">社交媒体</p>
                        </div>
                        <button class="text-primary hover:text-primary/80 transition-colors">
                            <i class="fa-solid fa-plus"></i>
                        </button>
                    </div>
                    
                    <div class="app-card-hover flex items-center p-3 rounded-lg cursor-pointer">
                        <div class="w-10 h-10 rounded-lg bg-[#1877F2] flex items-center justify-center mr-3">
                            <i class="fa-brands fa-facebook text-white"></i>
                        </div>
                        <div class="flex-grow">
                            <h4 class="font-medium">Facebook</h4>
                            <p class="text-xs text-gray-500">社交媒体</p>
                        </div>
                        <button class="text-primary hover:text-primary/80 transition-colors">
                            <i class="fa-solid fa-plus"></i>
                        </button>
                    </div>
                    
                    <div class="app-card-hover flex items-center p-3 rounded-lg cursor-pointer">
                        <div class="w-10 h-10 rounded-lg bg-[#FF4500] flex items-center justify-center mr-3">
                            <i class="fa-brands fa-reddit text-white"></i>
                        </div>
                        <div class="flex-grow">
                            <h4 class="font-medium">Reddit</h4>
                            <p class="text-xs text-gray-500">社交新闻</p>
                        </div>
                        <button class="text-primary hover:text-primary/80 transition-colors">
                            <i class="fa-solid fa-plus"></i>
                        </button>
                    </div>
                </div>
                
                <div class="text-center mt-4">
                    <button class="text-primary hover:text-primary/80 transition-colors text-sm font-medium">
                        加载更多应用 <i class="fa-solid fa-chevron-down ml-1"></i>
                    </button>
                </div>
            </div>
        </section>
    </main>

    <!-- 悬浮按钮 -->
    <div class="fixed bottom-6 right-6 z-40">
        <button id="floatBtn" class="float-button bg-primary text-white w-14 h-14 rounded-full shadow-lg flex items-center justify-center">
            <i class="fa-solid fa-columns text-xl"></i>
        </button>
    </div>

    <!-- 悬浮菜单 -->
    <div id="floatMenu" class="fixed bottom-24 right-6 z-40 bg-white rounded-xl shadow-xl p-4 w-64 hidden animate-fade-in">
        <div class="flex justify-between items-center mb-3">
            <h3 class="font-medium">快速分屏</h3>
            <button id="closeMenu" class="text-gray-400 hover:text-gray-600 transition-colors">
                <i class="fa-solid fa-times"></i>
            </button>
        </div>
        <div class="space-y-3">
            <div class="flex items-center p-2 rounded-lg hover:bg-gray-50 transition-colors cursor-pointer">
                <div class="w-8 h-8 rounded-lg bg-[#FF5252] flex items-center justify-center mr-3">
                    <i class="fa-brands fa-whatsapp text-white text-sm"></i>
                </div>
                <div class="flex-grow">
                    <h4 class="text-sm font-medium">WhatsApp</h4>
                </div>
                <div class="w-8 h-8 rounded-lg bg-gray-200 flex items-center justify-center">
                    <i class="fa-solid fa-arrow-right text-gray-500 text-sm"></i>
                </div>
            </div>
            
            <div class="flex items-center p-2 rounded-lg hover:bg-gray-50 transition-colors cursor-pointer">
                <div class="w-8 h-8 rounded-lg bg-[#4285F4] flex items-center justify-center mr-3">
                    <i class="fa-brands fa-chrome text-white text-sm"></i>
                </div>
                <div class="flex-grow">
                    <h4 class="text-sm font-medium">Chrome</h4>
                </div>
                <div class="w-8 h-8 rounded-lg bg-gray-200 flex items-center justify-center">
                    <i class="fa-solid fa-arrow-right text-gray-500 text-sm"></i>
                </div>
            </div>
            
            <div class="border-t border-gray-100 my-2 pt-2">
                <button class="w-full bg-primary/10 text-primary py-2 rounded-lg hover:bg-primary/20 transition-colors text-sm font-medium">
                    自定义分屏组合
                </button>
            </div>
        </div>
    </div>

    <!-- 分屏预览模态框 -->
    <div id="splitScreenModal" class="fixed inset-0 bg-black/50 z-50 hidden items-center justify-center glass">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-4xl mx-4 animate-fade-in">
            <div class="p-6">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-xl font-bold text-dark">分屏预览</h3>
                    <button id="closeModal" class="text-gray-400 hover:text-gray-600 transition-colors">
                        <i class="fa-solid fa-times text-xl"></i>
                    </button>
                </div>
                
                <div class="flex flex-col md:flex-row h-80 rounded-xl overflow-hidden shadow-inner mb-6">
                    <div class="w-full md:w-1/2 bg-[#FF5252] flex flex-col">
                        <div class="h-10 bg-[#E53935] flex items-center px-3">
                            <div class="flex space-x-1">
                                <div class="w-3 h-3 rounded-full bg-[#FF5F56]"></div>
                                <div class="w-3 h-3 rounded-full bg-[#FFBD2E]"></div>
                                <div class="w-3 h-3 rounded-full bg-[#27C93F]"></div>
                            </div>
                            <h4 class="ml-3 text-white text-sm font-medium">WhatsApp</h4>
                        </div>
                        <div class="flex-grow p-4">
                            <div class="bg-white/10 rounded-lg p-3 mb-3 animate-pulse-light">
                                <p class="text-white text-sm">你好！今天过得怎么样？</p>
                            </div>
                            <div class="bg-white/10 rounded-lg p-3 mb-3 animate-pulse-light" style="animation-delay: 0.2s">
                                <p class="text-white text-sm">我正在使用分屏功能，非常方便！</p>
                            </div>
                            <div class="bg-white/10 rounded-lg p-3 animate-pulse-light" style="animation-delay: 0.4s">
                                <p class="text-white text-sm">是的，我也觉得这个功能很棒！</p>
                            </div>
                        </div>
                    </div>
                    
                    <div class="w-full md:w-1/2 bg-[#4285F4] flex flex-col">
                        <div class="h-10 bg-[#3367D6] flex items-center px-3">
                            <div class="flex space-x-1">
                                <div class="w-3 h-3 rounded-full bg-[#FF5F56]"></div>
                                <div class="w-3 h-3 rounded-full bg-[#FFBD2E]"></div>
                                <div class="w-3 h-3 rounded-full bg-[#27C93F]"></div>
                            </div>
                            <h4 class="ml-3 text-white text-sm font-medium">Chrome</h4>
                        </div>
                        <div class="flex-grow p-4">
                            <div class="bg-white/10 rounded-lg p-3 h-full animate-pulse-light">
                                <p class="text-white text-sm">https://www.example.com</p>
                                <div class="mt-4 h-[85%] bg-white/5 rounded-lg"></div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="flex justify-between items-center">
                    <div class="flex items-center space-x-3">
                        <button class="flex items-center text-gray-600 hover:text-primary transition-colors">
                            <i class="fa-solid fa-arrows-alt-h mr-2"></i>
                            <span class="text-sm">调整比例</span>
                        </button>
                        <button class="flex items-center text-gray-600 hover:text-primary transition-colors">
                            <i class="fa-solid fa-exchange-alt mr-2"></i>
                            <span class="text-sm">交换位置</span>
                        </button>
                    </div>
                    <button class="bg-primary text-white px-6 py-2 rounded-lg hover:bg-primary/90 transition-colors shadow-md float-button">
                        启动分屏
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- 底部导航栏 -->
    <footer class="bg-white shadow-inner py-4 border-t border-gray-100 sticky bottom-0 z-30">
        <div class="container mx-auto px-4">
            <div class="flex justify-around">
                <a href="#" class="flex flex-col items-center text-primary">
                    <i class="fa-solid fa-columns text-xl"></i>
                    <span class="text-xs mt-1">分屏</span>
                </a>
                <a href="#" class="flex flex-col items-center text-gray-400 hover:text-primary transition-colors">
                    <i class="fa-solid fa-clock-rotate-left text-xl"></i>
                    <span class="text-xs mt-1">历史</span>
                </a>
                <a href="#" class="flex flex-col items-center text-gray-400 hover:text-primary transition-colors">
                    <i class="fa-solid fa-star text-xl"></i>
                    <span class="text-xs mt-1">收藏</span>
                </a>
                <a href="#" class="flex flex-col items-center text-gray-400 hover:text-primary transition-colors">
                    <i class="fa-solid fa-sliders-h text-xl"></i>
                    <span class="text-xs mt-1">设置</span>
                </a>
            </div>
        </div>
    </footer>

    <script>
        // 浮动按钮和菜单交互
        const floatBtn = document.getElementById('floatBtn');
        const floatMenu = document.getElementById('floatMenu');
        const closeMenu = document.getElementById('closeMenu');
        
        floatBtn.addEventListener('click', function(e) {
            createRipple(e);
            floatMenu.classList.toggle('hidden');
        });
        
        closeMenu.addEventListener('click', function() {
            floatMenu.classList.add('hidden');
        });
        
        // 模态框交互
        const splitScreenModal = document.getElementById('splitScreenModal');
        const closeModal = document.getElementById('closeModal');
        const startSplitButtons = document.querySelectorAll('button:contains("启动分屏")');
        
        startSplitButtons.forEach(button => {
            button.addEventListener('click', function() {
                splitScreenModal.style.display = 'flex';
            });
        });
        
        closeModal.addEventListener('click', function() {
            splitScreenModal.style.display = 'none';
        });
        
        // 点击模态框外部关闭
        splitScreenModal.addEventListener('click', function(e) {
            if (e.target === splitScreenModal) {
                splitScreenModal.style.display = 'none';
            }
        });
        
        // 波纹效果
        function createRipple(event) {
            const button = event.currentTarget;
            const circle = document.createElement('span');
            const diameter = Math.max(button.clientWidth, button.clientHeight);
            const radius = diameter / 2;
            
            circle.style.width = circle.style.height = `${diameter}px`;
            circle.style.left = `${event.clientX - button.getBoundingClientRect().left - radius}px`;
            circle.style.top = `${event.clientY - button.getBoundingClientRect().top - radius}px`;
            circle.classList.add('ripple');
            
            const ripple = button.getElementsByClassName('ripple')[0];
            if (ripple) {
                ripple.remove();
            }
            
            button.appendChild(circle);
        }
        
        // 应用卡片点击事件
        const appCards = document.querySelectorAll('.app-card-hover');
        appCards.forEach(card => {
            card.addEventListener('click', function() {
                // 这里可以添加应用选择逻辑
                console.log('Selected app:', card.querySelector('p').textContent);
            });
        });
    </script>
</body>
</html>
    # -
一键分屏工具
