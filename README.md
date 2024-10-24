<html>
<head>
    <title>NFT Gallery Card</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
/* Root Variables */
:root {
    --primary-color: #8b5cf6;
    --primary-hover: #7c3aed;
    --accent-color: #c4b5fd;
    --background-dark: #1a1b26;
    --background-light: #2c3e50;
    --text-light: #e2e8f0;
    --text-dark: #1f2937;
    --gold: #ffd700;
    --marble: linear-gradient(45deg, #f3f4f6 0%, #e5e7eb 100%);
    --shadow-sm: 0 2px 4px rgba(0, 0, 0, 0.1);
    --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
    --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);
    --transition-fast: 0.2s ease;
    --transition-normal: 0.3s ease;
    --transition-slow: 0.5s ease;
}

/* Reset and Base Styles */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: var(--background-dark);
    padding: 0;
    margin: 0;
    overflow: hidden;
    color: var(--text-light);
}

/* Gallery Card Container */
.gallery-card {
    width: 100vw;
    height: 100vh;
    background: var(--background-dark);
    position: relative;
    overflow: hidden;
}

/* 3D Gallery Container */
.gallery-container {
    width: 100%;
    height: 100%;
    perspective: 2000px;
    perspective-origin: 50% 50%;
}

.room {
    width: 100%;
    height: 100%;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Wall Styling */
.wall {
    position: absolute;
    width: 100%;
    height: 100%;
    display: grid;
    padding: max(20px, 4vw);
    background: linear-gradient(180deg, var(--background-light) 0%, var(--background-dark) 100%);
    backface-visibility: hidden;
}

/* Responsive Wall Transformations */
@media (min-width: 1024px) {
    .wall {
        grid-template-columns: repeat(3, 1fr);
        gap: 30px;
    }
    .front-wall { transform: translateZ(-500px); }
    .back-wall { transform: translateZ(500px) rotateY(180deg); }
    .left-wall { transform: translateX(-500px) rotateY(90deg); }
    .right-wall { transform: translateX(500px) rotateY(-90deg); }
}

@media (min-width: 768px) and (max-width: 1023px) {
    .wall {
        grid-template-columns: repeat(2, 1fr);
        gap: 25px;
    }
    .front-wall { transform: translateZ(-400px); }
    .back-wall { transform: translateZ(400px) rotateY(180deg); }
    .left-wall { transform: translateX(-400px) rotateY(90deg); }
    .right-wall { transform: translateX(400px) rotateY(-90deg); }
}

@media (max-width: 767px) {
    .wall {
        grid-template-columns: 1fr;
        gap: 20px;
    }
    .front-wall { transform: translateZ(-300px); }
    .back-wall { transform: translateZ(300px) rotateY(180deg); }
    .left-wall { transform: translateX(-300px) rotateY(90deg); }
    .right-wall { transform: translateX(300px) rotateY(-90deg); }
}

/* Artwork Frame Styling */
.artwork-frame {
    background: var(--marble);
    border-radius: 20px;
    padding: 12px;
    box-shadow: var(--shadow-lg);
    aspect-ratio: 1;
    transform: scale(0.98);
    transition: transform var(--transition-normal);
}

.artwork-frame:hover {
    transform: scale(1);
}

/* Artwork Container */
.artwork {
    width: 100%;
    height: 100%;
    position: relative;
    cursor: pointer;
    border-radius: 15px;
    overflow: hidden;
    background: var(--background-dark);
}

/* Artwork Image */
.artwork img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform var(--transition-slow);
}

.artwork:hover img {
    transform: scale(1.05);
}

/* Price Tag Styling */
.price-tag {
    position: absolute;
    top: 15px;
    right: 15px;
    background: rgba(0, 0, 0, 0.8);
    color: var(--gold);
    padding: 8px 15px;
    border-radius: 20px;
    font-size: clamp(12px, 2vw, 16px);
    z-index: 1;
    backdrop-filter: blur(5px);
    box-shadow: var(--shadow-sm);
}

/* Artwork Information */
.artwork-info {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.85);
    padding: 15px;
    transform: translateY(100%);
    transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    backdrop-filter: blur(5px);
}

.artwork:hover .artwork-info {
    transform: translateY(0);
}

.artwork-info h3 {
    font-size: clamp(16px, 2.5vw, 20px);
    margin-bottom: 8px;
}

.artwork-info p {
    font-size: clamp(12px, 2vw, 14px);
    opacity: 0.8;
}

/* Control Buttons */
.controls {
    position: fixed;
    bottom: 30px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 15px;
    z-index: 100;
}

.control-btn {
    padding: clamp(8px, 2vw, 12px) clamp(15px, 3vw, 25px);
    background: var(--primary-color);
    color: var(--text-light);
    border: none;
    border-radius: 25px;
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: clamp(14px, 2vw, 16px);
    transition: all var(--transition-normal);
    box-shadow: 0 4px 15px rgba(139, 92, 246, 0.3);
}

.control-btn:hover {
    background: var(--primary-hover);
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(139, 92, 246, 0.4);
}

/* Modal Styling */
.modal {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.9);
    z-index: 1000;
    justify-content: center;
    align-items: center;
    backdrop-filter: blur(8px);
}

.modal-content {
    background: var(--background-dark);
    padding: clamp(20px, 4vw, 30px);
    border-radius: 25px;
    max-width: min(90%, 600px);
    width: 100%;
    color: var(--text-light);
    position: relative;
    box-shadow: var(--shadow-lg);
}

/* Modal Close Button */
.close-modal {
    position: absolute;
    top: 15px;
    right: 15px;
    color: white;
    cursor: pointer;
    font-size: 24px;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.1);
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all var(--transition-normal);
}

.close-modal:hover {
    background: rgba(255, 255, 255, 0.2);
    transform: rotate(90deg);
}

/* Buy Button */
.buy-btn {
    background: var(--primary-color);
    color: var(--text-light);
    padding: 15px;
    border: none;
    border-radius: 15px;
    cursor: pointer;
    margin-top: 20px;
    width: 100%;
    font-size: 16px;
    transition: all var(--transition-normal);
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
}

.buy-btn:hover {
    background: var(--primary-hover);
    transform: translateY(-2px);
}

/* Loading Screen */
#loading-screen {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: var(--background-dark);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    z-index: 9999;
}

.loader {
    width: 50px;
    height: 50px;
    border: 4px solid var(--accent-color);
    border-top: 4px solid var(--gold);
    border-radius: 50%;
    animation: spin 1s linear infinite;
}

/* Loading Animation */
@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

/* Mobile Touch Optimization */
@media (hover: none) {
    .artwork:hover img {
        transform: none;
    }
    
    .artwork-info {
        transform: translateY(0);
        background: rgba(0, 0, 0, 0.7);
        height: auto;
        max-height: 40%;
    }
}

/* Print Styles */
@media print {
    .controls,
    .modal,
    #loading-screen {
        display: none !important;
    }
    
    .gallery-card {
        height: auto;
        overflow: visible;
    }
    
    .wall {
        position: relative;
        transform: none !important;
        page-break-after: always;
    }
}

/* High Contrast Mode */
@media (prefers-contrast: high) {
    :root {
        --primary-color: #0066cc;
        --primary-hover: #004c99;
        --accent-color: #ffffff;
        --background-dark: #000000;
        --background-light: #333333;
        --text-light: #ffffff;
    }
}

/* Reduced Motion */
@media (prefers-reduced-motion: reduce) {
    *,
    *::before,
    *::after {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
        scroll-behavior: auto !important;
    }
}
    </style>
</head>
<body>
    <div class="gallery-card">
        <div id="loading-screen">
            <div class="loader"></div>
            <div style="color: white; margin-top: 10px;">Loading Gallery...</div>
        </div>

        <div class="gallery-container">
            <div class="room">
                <div class="wall front-wall">
                    <div class="artwork-frame">
                        <div class="artwork" data-id="1">
                            <div class="price-tag">2.5 ETH</div>
                            <img src="https://i.ibb.co/yFNbRqF/Whats-App-Image-2024-10-20-at-09-16-07-510389dd.jpg" alt="NFT 1">
                            <div class="artwork-info">
                                <h3 style="color: white;">Digital Dreams #1</h3>
                                <p style="color: #ccc;">A mesmerizing digital artwork</p>
                            </div>
                        </div>
                    </div>
                    <div class="artwork-frame">
                        <div class="artwork" data-id="2">
                            <div class="price-tag">3.8 ETH</div>
                            <img src="https://i.ibb.co/7vkRDZv/Whats-App-Image-2024-10-20-at-09-16-08-0798f91f.jpg" alt="NFT 2">
                            <div class="artwork-info">
                                <h3 style="color: white;">Cosmic Voyage #4</h3>
                                <p style="color: #ccc;">Journey through digital space</p>
                            </div>
                        </div>
                    </div>
                    <div class="artwork-frame">
                        <div class="artwork" data-id="3">
                            <div class="price-tag">5.2 ETH</div>
                            <img src="https://i.ibb.co/7vkRDZv/Whats-App-Image-2024-10-20-at-09-16-08-0798f91f.jpg" alt="NFT 3">
                            <div class="artwork-info">
                                <h3 style="color: white;">Neural Nexus</h3>
                                <p style="color: #ccc;">AI-generated masterpiece</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="wall back-wall">
                    <div class="artwork-frame">
                        <div class="artwork" data-id="4">
                            <div class="price-tag">4.1 ETH</div>
                            <img src="/api/placeholder/400/400" alt="NFT 4">
                            <div class="artwork-info">
                                <h3 style="color: white;">Quantum Fragments</h3>
                                <p style="color: #ccc;">Digital quantum art exploration</p>
                            </div>
                        </div>
                    </div>
                    <div class="artwork-frame">
                        <div class="artwork" data-id="5">
                            <div class="price-tag">3.2 ETH</div>
                            <img src="/api/placeholder/400/400" alt="NFT 5">
                            <div class="artwork-info">
                                <h3 style="color: white;">Cyber Synthesis</h3>
                                <p style="color: #ccc;">Fusion of digital and analog</p>
                            </div>
                        </div>
                    </div>
                    <div class="artwork-frame">
                        <div class="artwork" data-id="6">
                            <div class="price-tag">6.7 ETH</div>
                            <img src="/api/placeholder/400/400" alt="NFT 6">
                            <div class="artwork-info">
                                <h3 style="color: white;">Meta Horizons</h3>
                                <p style="color: #ccc;">Exploring virtual landscapes</p>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="wall left-wall">
                    <div class="artwork-frame">
                        <div class="artwork" data-id="7">
                            <div class="price-tag">2.9 ETH</div>
                            <img src="/api/placeholder/400/400" alt="NFT 7">
                            <div class="artwork-info">
                                <h3 style="color: white;">Digital Genesis</h3>
                                <p style="color: #ccc;">Birth of digital consciousness</p>
                            </div>
                        </div>
                    </div>
                    <div class="artwork-frame">
                        <div class="artwork" data-id="8">
                            <div class="price-tag">4.5 ETH</div>
                            <img src="/api/placeholder/400/400" alt="NFT 8">
                            <div class="artwork-info">
                                <h3 style="color: white;">Neon Dreams</h3>
                                <p style="color: #ccc;">Cyberpunk-inspired artwork</p>
                            </div>
                        </div>
                    </div>
                    <div class="artwork-frame">
                        <div class="artwork" data-id="9">
                            <div class="price-tag">5.8 ETH</div>
                            <img src="/api/placeholder/400/400" alt="NFT 9">
                            <div class="artwork-info">
                                <h3 style="color: white;">Virtual Echo</h3>
                                <p style="color: #ccc;">Resonating through the metaverse</p>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="wall right-wall">
                    <div class="artwork-frame">
                        <div class="artwork" data-id="10">
                            <div class="price-tag">3.7 ETH</div>
                            <img src="/api/placeholder/400/400" alt="NFT 10">
                            <div class="artwork-info">
                                <h3 style="color: white;">Crypto Canvas</h3>
                                <p style="color: #ccc;">Blockchain-inspired art</p>
                            </div>
                        </div>
                    </div>
                    <div class="artwork-frame">
                        <div class="artwork" data-id="11">
                            <div class="price-tag">4.9 ETH</div>
                            <img src="/api/placeholder/400/400" alt="NFT 11">
                            <div class="artwork-info">
                                <h3 style="color: white;">Digital Dystopia</h3>
                                <p style="color: #ccc;">Future visions reimagined</p>
                            </div>
                        </div>
                    </div>
                    <div class="artwork-frame">
                        <div class="artwork" data-id="12">
                            <div class="price-tag">7.1 ETH</div>
                            <img src="/api/placeholder/400/400" alt="NFT 12">
                            <div class="artwork-info">
                                <h3 style="color: white;">Pixel Paradise</h3>
                                <p style="color: #ccc;">Retro-futuristic masterpiece</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="controls">
            <button class="control-btn" onclick="rotate('left')">
                <i class="fas fa-chevron-left"></i> Rotate Left</button>
                <button class="control-btn" onclick="rotate('right')">
                    Rotate Right <i class="fas fa-chevron-right"></i>
                </button>
            </div>
        </div>
    
        <div class="modal" id="nftModal">
            <div class="modal-content">
                <span class="close-modal">&times;</span>
                <img src="" alt="NFT Preview" style="width: 100%; border-radius: 10px; margin-bottom: 15px;">
                <h2 style="color: var(--gold);"></h2>
                <p class="artist-info" style="color: #ccc; margin: 10px 0;"></p>
                <p class="description" style="margin-bottom: 10px;"></p>
                <p class="price" style="color: var(--gold);"></p>
                <button class="buy-btn">
                    <i class="fas fa-shopping-cart"></i> Buy Now
                </button>
            </div>
        </div>
    
        <script>
            let currentRotation = 0;
            const room = document.querySelector('.room');
            const modal = document.getElementById('nftModal');
    
            // Extended NFT Data
            const nftData = {
                1: {
                    title: "Digital Dreams #1",
                    artist: "CryptoArtist",
                    description: "A mesmerizing digital artwork that explores the boundaries between reality and imagination.",
                    price: "2.5 ETH",
                    created: "2024-03-15"
                },
                2: {
                    title: "Cosmic Voyage #4",
                    artist: "NeuroArtisan",
                    description: "An interstellar journey through digital realms.",
                    price: "3.8 ETH",
                    created: "2024-03-18"
                },
                3: {
                    title: "Neural Nexus",
                    artist: "AICreator",
                    description: "AI-generated masterpiece pushing boundaries.",
                    price: "5.2 ETH",
                    created: "2024-03-20"
                },
                4: {
                    title: "Quantum Fragments",
                    artist: "QuantumArtist",
                    description: "Digital quantum art exploring parallel realities.",
                    price: "4.1 ETH",
                    created: "2024-03-22"
                },
                5: {
                    title: "Cyber Synthesis",
                    artist: "TechnoCreative",
                    description: "A unique fusion of digital and analog aesthetics.",
                    price: "3.2 ETH",
                    created: "2024-03-23"
                },
                6: {
                    title: "Meta Horizons",
                    artist: "VirtualVanguard",
                    description: "Exploring the boundaries of virtual landscapes.",
                    price: "6.7 ETH",
                    created: "2024-03-24"
                },
                7: {
                    title: "Digital Genesis",
                    artist: "CryptoCreator",
                    description: "Witnessing the birth of digital consciousness.",
                    price: "2.9 ETH",
                    created: "2024-03-25"
                },
                8: {
                    title: "Neon Dreams",
                    artist: "CyberArtist",
                    description: "Cyberpunk-inspired artwork from the future.",
                    price: "4.5 ETH",
                    created: "2024-03-26"
                },
                9: {
                    title: "Virtual Echo",
                    artist: "MetaCreator",
                    description: "Resonating through the depths of the metaverse.",
                    price: "5.8 ETH",
                    created: "2024-03-27"
                },
                10: {
                    title: "Crypto Canvas",
                    artist: "BlockchainArtist",
                    description: "Art inspired by the blockchain revolution.",
                    price: "3.7 ETH",
                    created: "2024-03-28"
                },
                11: {
                    title: "Digital Dystopia",
                    artist: "FutureVision",
                    description: "A glimpse into tomorrow's digital landscape.",
                    price: "4.9 ETH",
                    created: "2024-03-29"
                },
                12: {
                    title: "Pixel Paradise",
                    artist: "RetroFuturist",
                    description: "Where retro meets the future of digital art.",
                    price: "7.1 ETH",
                    created: "2024-03-30"
                }
            };
    
            // Hide loading screen
            window.addEventListener('load', () => {
                setTimeout(() => {
                    const loadingScreen = document.getElementById('loading-screen');
                    loadingScreen.style.transition = 'opacity 0.5s ease';
                    loadingScreen.style.opacity = '0';
                    setTimeout(() => {
                        loadingScreen.style.display = 'none';
                    }, 500);
                }, 1500);
            });
    
            function rotate(direction) {
                currentRotation += direction === 'left' ? -90 : 90;
                room.style.transform = `rotateY(${currentRotation}deg)`;
            }
    
            // Setup artwork click handlers
            document.querySelectorAll('.artwork').forEach(artwork => {
                artwork.addEventListener('click', function() {
                    const id = this.dataset.id;
                    const nft = nftData[id];
                    
                    const modalContent = modal.querySelector('.modal-content');
                    modalContent.querySelector('img').src = this.querySelector('img').src;
                    modalContent.querySelector('h2').textContent = nft.title;
                    modalContent.querySelector('.artist-info').textContent = `By ${nft.artist} • Created ${nft.created}`;
                    modalContent.querySelector('.description').textContent = nft.description;
                    modalContent.querySelector('.price').textContent = `Price: ${nft.price}`;
                    
                    modal.style.display = 'flex';
                });
            });
    
            // Close modal handlers
            document.querySelector('.close-modal').addEventListener('click', () => {
                modal.style.display = 'none';
            });
    
            window.onclick = function(event) {
                if (event.target == modal) {
                    modal.style.display = 'none';
                }
            }
    
            // Buy button handler
            document.querySelector('.buy-btn').addEventListener('click', function() {
                this.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Connecting Wallet...';
                this.style.opacity = '0.7';
                this.style.cursor = 'not-allowed';
                
                setTimeout(() => {
                    alert('Connecting to NFT marketplace...');
                    this.innerHTML = '<i class="fas fa-shopping-cart"></i> Buy Now';
                    this.style.opacity = '1';
                    this.style.cursor = 'pointer';
                }, 1500);
            });
    
            // Keyboard controls
            document.addEventListener('keydown', function(e) {
                if (e.key === 'ArrowLeft') {
                    rotate('left');
                } else if (e.key === 'ArrowRight') {
                    rotate('right');
                } else if (e.key === 'Escape' && modal.style.display === 'flex') {
                    modal.style.display = 'none';
                }
            });
    
            // Touch controls
            let touchStartX = 0;
            let touchEndX = 0;
            const SWIPE_THRESHOLD = 50;
    
            document.addEventListener('touchstart', e => {
                touchStartX = e.touches[0].clientX;
            });
    
            document.addEventListener('touchmove', e => {
                touchEndX = e.touches[0].clientX;
            });
    
            document.addEventListener('touchend', () => {
                const swipeDistance = touchEndX - touchStartX;
                
                if (Math.abs(swipeDistance) > SWIPE_THRESHOLD) {
                    if (swipeDistance > 0) {
                        rotate('left');
                    } else {
                        rotate('right');
                    }
                }
                
                // Reset values
                touchStartX = 0;
                touchEndX = 0;
            });
        </script>
    </body>
