<template>
    <div class="cover-container">
        <!-- Scene 1: Initial gift/rose image -->
        <div v-if="!showBouquet" class="initial-scene">
            <div class="gift-container" @click="showFlowerBouquet">
                <img :src="giftImage" alt="Gift" class="gift-image animate-float" />
                <p class="tap-text">Chạm vào ảnh để khám phá</p>
            </div>
        </div>

        <!-- Scene 2: Flower bouquet with letter -->
        <div v-else-if="!hasSeenAllMessages" class="bouquet-scene">
            <div class="celebration-text" :class="{ 'show': showCelebration }">
                <h1>🌹 Chúc mừng Ngày Phụ Nữ Việt Nam! 🌹</h1>
                <p class="celebration-subtitle">20 tháng 10 - Ngày của những người phụ nữ tuyệt vời</p>
            </div>

            <div class="bouquet-container" :class="{ 'show': showBouquet }">
                <img :src="bouquetImage" alt="Bouquet of roses" class="bouquet-image animate-gentle-sway" />
                <div 
                    v-if="showButton" 
                    class="love-letter-btn" 
                    :class="{ 'show': showButton, 'disabled': hasSeenAllMessages }"
                    @click="handleButtonClick"
                >
                    <div class="btn-background">
                        <div class="btn-layer layer-1"></div>
                        <div class="btn-layer layer-2"></div>
                        <div class="btn-layer layer-3"></div>
                    </div>
                    <div class="btn-content">
                        <div class="btn-icon">
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                                <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                                <polyline points="22,6 12,13 2,6"/>
                            </svg>
                            <div class="heart-float">�</div>
                        </div>
                        <span class="btn-text">{{ buttonText }}</span>
                        <div class="btn-glow"></div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Scene 3: Final thank you scene -->
        <div v-else class="final-scene">
            <div class="final-container">
                <img :src="finalImage" alt="Thank you" class="final-image animate-float" />
                <div class="final-text">
                    <h1>🌹 Xiaxia🌹</h1>
                    <p class="final-subtitle">Live đều đặn, ngủ sớm ạ!</p>
                </div>
            </div>
        </div>

        <!-- Modal with sweet message -->
        <div v-if="showModal" class="modal-overlay" @click="closeModal">
            <div class="modal-content" @click.stop :class="{ 'show': showModal }">
                <button class="close-btn" @click="closeModal">&times;</button>
                <div class="modal-body">
                    <h2 class="modal-title">{{ currentMessage.title }}</h2>
                    
                    <!-- Image gallery -->
                    <div v-if="currentMessage.images" class="image-gallery">
                        <img 
                            v-for="(image, index) in currentMessage.images" 
                            :key="index"
                            :src="image" 
                            :alt="`Image ${index + 1}`"
                            class="modal-image"
                        />
                    </div>
                    
                    <p class="modal-message">
                        {{ currentMessage.content }}
                    </p>
                    <div class="flower-decoration">{{ currentMessage.decoration }}</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Cover',
    data() {
        return {
            showBouquet: false,
            showCelebration: false,
            showButton: false,
            showModal: false,
            currentMessageIndex: 0,
            giftImage: new URL('../assets/img/me.jpeg', import.meta.url).href,
            bouquetImage: new URL('../assets/img/flower.png', import.meta.url).href,
            finalImage: new URL('../assets/img/xiaxia.jpg', import.meta.url).href,
            messages: [
                {
                    title: "💕 Lời chúc đầu tiên 💕",
                    content: "Chúc bạn 20/10 thật rực rỡ, xinh đẹp và luôn được yêu thương như chính cách bạn lan tỏa năng lượng tích cực cho mọi người.",
                    decoration: "🌹🌸🌹",
                    images: [
                        new URL('../assets/img/baesulinh.jpeg', import.meta.url).href,
                        new URL('../assets/img/foryou.jpeg', import.meta.url).href
                    ]
                },
                {
                    title: "🌺 Lời chúc thứ hai 🌺",
                    content: "Chúc bạn luôn tươi như nắng sớm, rạng rỡ như đóa hoa vừa nở và đủ mạnh mẽ để không cần ai tưới vẫn tự tỏa hương",
                    decoration: "🌺✨🌺",
                    images: [
                        new URL('../assets/img/ok.jpeg', import.meta.url).href,
                        new URL('../assets/img/wow.jpeg', import.meta.url).href
                    ]
                },
                {
                    title: "🦋 Lời chúc thứ ba 🦋",
                    content: "Chúc bạn luôn gặp điều tốt lành, đủ dịu dàng để khiến người khác ấm lòng và đủ kiên cường để khiến chính mình tự hào",
                    decoration: "🦋💖🦋",
                    images: [
                        new URL('../assets/img/binhtinhsong.jpeg', import.meta.url).href,
                        new URL('../assets/img/you.jpeg', import.meta.url).href
                    ]
                },
                {
                    title: "🌟 Lời chúc cuối nè 🌟",
                    content: "Chúc bạn 20/10 tràn đầy niềm vui, lúc nào cũng được yêu thương, và dù có bận đến đâu vẫn biết cách khiến bản thân xinh đẹp, an yên",
                    decoration: "🌟💫🌟",
                    images: [
                        new URL('../assets/img/kpi.jpeg', import.meta.url).href,
                        new URL('../assets/img/no.jpeg', import.meta.url).href
                    ]
                }
            ]
        }
    },
    computed: {
        buttonText() {
            if (this.currentMessageIndex === 0) {
                return "Thư Yêu Thương";
            } else if (this.currentMessageIndex < this.messages.length) {
                return "Còn tiếp nè";
            } else {
                return "Hết rồi nhen";
            }
        },
        currentMessage() {
            // Đảm bảo hiển thị đúng tin nhắn theo index thực tế
            const messageIndex = Math.max(0, this.currentMessageIndex - 1);
            if (messageIndex >= this.messages.length) {
                return this.messages[this.messages.length - 1];
            }
            return this.messages[messageIndex];
        },
        hasSeenAllMessages() {
            return this.currentMessageIndex >= this.messages.length;
        }
    },
    methods: {
        showFlowerBouquet() {
            this.showBouquet = true;

            // Show celebration text after bouquet animation starts
            setTimeout(() => {
                this.showCelebration = true;
            }, 800);

            // Show button after 5 seconds
            setTimeout(() => {
                this.showButton = true;
            }, 3000);
        },

        handleButtonClick() {
            // Nếu đã xem hết tin nhắn thì không làm gì
            if (this.hasSeenAllMessages) {
                return;
            }
            
            this.openModal();
        },

        openModal() {
            this.showModal = true;
            document.body.style.overflow = 'hidden';
            
            // Tăng currentMessageIndex để hiển thị tin nhắn tiếp theo (nhưng không vượt quá)
            if (this.currentMessageIndex < this.messages.length) {
                this.currentMessageIndex++;
            }
        },

        closeModal() {
            this.showModal = false;
            document.body.style.overflow = 'auto';
        }
    },
    
    // Debug - có thể xóa sau
    watch: {
        currentMessageIndex(newVal) {
            console.log(`Current message index: ${newVal}, Total messages: ${this.messages.length}`);
            console.log(`Button text should be: ${this.buttonText}`);
            console.log(`Has seen all messages: ${this.hasSeenAllMessages}`);
        }
    }
}
</script>

<style scoped>
.cover-container {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    padding: 20px;
    background: linear-gradient(135deg, #ffeef8 0%, #fff0f5 50%, #ffe4e6 100%);
}

/* Initial Scene Styles */
.initial-scene {
    text-align: center;
    animation: fadeIn 1s ease-in-out;
}

.gift-container {
    cursor: pointer;
    transition: transform 0.3s ease;
}

.gift-container:hover {
    transform: scale(1.05);
}

.gift-image {
    width: 300px;
    height: 300px;
    object-fit: cover;
    border-radius: 20px;
    box-shadow: 0 15px 35px rgba(255, 105, 180, 0.3);
    border: 3px solid #ffb6c1;
    transition: all 0.3s ease;
}

.gift-image:hover {
    box-shadow: 0 20px 40px rgba(255, 105, 180, 0.4);
    transform: translateY(-5px);
}

.animate-float {
    animation: float 3s ease-in-out infinite;
}

.tap-text {
    margin-top: 20px;
    font-size: 18px;
    color: #d63384;
    font-weight: 600;
    animation: pulse 2s infinite;
    font-family: 'Poppins', sans-serif;
}

/* Bouquet Scene Styles */
.bouquet-scene {
    text-align: center;
    width: 100%;
}

/* Final Scene Styles */
.final-scene {
    text-align: center;
    width: 100%;
    animation: fadeIn 1.5s ease-in-out;
}

.final-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 30px;
}

.final-image {
    width: 350px;
    height: 350px;
    object-fit: cover;
    border-radius: 25px;
    box-shadow: 0 20px 50px rgba(255, 105, 180, 0.4);
    border: 4px solid #ffb6c1;
    transition: all 0.3s ease;
}

.final-image:hover {
    transform: scale(1.05);
    box-shadow: 0 25px 60px rgba(255, 105, 180, 0.6);
}

.final-text {
    animation: slideUp 1s ease-out 0.5s both;
}

.final-text h1 {
    font-family: 'Dancing Script', cursive;
    font-size: 2.8rem;
    font-weight: 700;
    color: #d63384;
    text-shadow: 2px 2px 4px rgba(255, 182, 193, 0.5);
    margin-bottom: 15px;
    background: linear-gradient(45deg, #d63384, #ff69b4, #ffc0cb);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
}

.final-subtitle {
    font-family: 'Poppins', sans-serif;
    font-size: 1.2rem;
    color: #666;
    font-weight: 500;
    line-height: 1.5;
    max-width: 600px;
    margin: 0 auto;
}

.celebration-text {
    margin-bottom: 30px;
    opacity: 0;
    transform: translateY(-30px);
    transition: all 0.8s ease;
}

.celebration-text.show {
    opacity: 1;
    transform: translateY(0);
}

.celebration-text h1 {
    font-family: 'Dancing Script', cursive;
    font-size: 2.5rem;
    font-weight: 700;
    color: #d63384;
    text-shadow: 2px 2px 4px rgba(255, 182, 193, 0.5);
    margin-bottom: 10px;
}

.bouquet-container {
    position: relative;
    display: inline-block;
    opacity: 0;
    transform: scale(0.3);
    transition: all 1s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.bouquet-container.show {
    opacity: 1;
    transform: scale(1);
}

.bouquet-image {
    width: 300px;
    height: 400px;
    object-fit: cover;
    /* border-radius: 25px; */
    /* box-shadow: 0 20px 40px rgba(255, 105, 180, 0.4); */
    /* border: 4px solid #ffb6c1; */
}

.love-letter-btn {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    cursor: pointer;
    opacity: 0;
    transition: all 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
    border: none;
    outline: none;
    width: 180px;
    height: 60px;
    border-radius: 35px;
    overflow: hidden;
    font-family: 'Poppins', sans-serif;
}

.love-letter-btn.show {
    opacity: 1;
    animation: magicalEntrance 1.2s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.love-letter-btn:hover {
    transform: translate(-50%, -50%) scale(1.08);
}

.love-letter-btn:active {
    transform: translate(-50%, -50%) scale(1.02);
}

.love-letter-btn.disabled {
    cursor: not-allowed;
    opacity: 0.6;
    filter: grayscale(50%);
}

.love-letter-btn.disabled:hover {
    transform: translate(-50%, -50%) scale(1);
}

.love-letter-btn.disabled .layer-1,
.love-letter-btn.disabled .layer-2,
.love-letter-btn.disabled .layer-3 {
    background: linear-gradient(45deg, #999, #bbb);
}

.love-letter-btn.disabled .sparkles {
    opacity: 0;
}

.btn-background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    border-radius: 35px;
}

.btn-layer {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 35px;
    transition: all 0.4s ease;
}

.layer-1 {
    background: linear-gradient(45deg, #ff6b9d, #ff8fab);
    opacity: 1;
}

.layer-2 {
    background: linear-gradient(135deg, #ff4081, #e91e63);
    opacity: 0;
}

.layer-3 {
    background: linear-gradient(225deg, #f06292, #ec407a);
    opacity: 0;
}

.love-letter-btn:hover .layer-1 {
    opacity: 0;
}

.love-letter-btn:hover .layer-2 {
    opacity: 1;
    animation: layerShift 2s ease-in-out infinite;
}

.love-letter-btn:hover .layer-3 {
    opacity: 0.7;
    animation: layerShift 2s ease-in-out infinite reverse;
}

.btn-content {
    position: relative;
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    width: 100%;
    height: 100%;
    color: white;
    text-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
}

.btn-icon {
    position: relative;
    width: 20px;
    height: 20px;
    color: white;
    filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));
}

.btn-icon svg {
    width: 100%;
    height: 100%;
    animation: iconFloat 2s ease-in-out infinite;
}

.heart-float {
    position: absolute;
    top: -5px;
    right: -5px;
    font-size: 12px;
    animation: heartPulse 1.5s ease-in-out infinite;
    opacity: 0.8;
}

.btn-text {
    font-size: 14px;
    font-weight: 600;
    letter-spacing: 0.5px;
    animation: textGlow 2s ease-in-out infinite;
}

.btn-glow {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 200%;
    height: 200%;
    background: radial-gradient(circle, rgba(255, 255, 255, 0.3) 0%, transparent 70%);
    opacity: 0;
    transition: opacity 0.3s ease;
}

.love-letter-btn:hover .btn-glow {
    opacity: 1;
    animation: glowPulse 1.5s ease-in-out infinite;
}

.sparkles {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.3s ease;
}

.love-letter-btn:hover .sparkles {
    opacity: 1;
}

.sparkle {
    position: absolute;
    font-size: 12px;
    animation: sparkleFloat 3s ease-in-out infinite;
}

.sparkle-1 {
    top: 10%;
    left: 15%;
    animation-delay: 0s;
}

.sparkle-2 {
    top: 20%;
    right: 10%;
    animation-delay: 0.5s;
}

.sparkle-3 {
    bottom: 15%;
    left: 20%;
    animation-delay: 1s;
}

.sparkle-4 {
    bottom: 10%;
    right: 15%;
    animation-delay: 1.5s;
}

.letter-envelope {
    font-size: 1.8rem;
    display: block;
    margin: 0;
    filter: drop-shadow(0 2px 4px rgba(255, 255, 255, 0.8));
}

.letter-text {
    font-size: 14px;
    color: #fff;
    font-weight: 600;
    font-family: 'Poppins', sans-serif;
    text-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
    margin: 0;
}

/* Modal Styles */
.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(255, 182, 193, 0.8);
    backdrop-filter: blur(5px);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
    animation: fadeIn 0.3s ease;
    padding: 20px;
}

.modal-content {
    background: linear-gradient(135deg, #fff, #fef7f7);
    border-radius: 25px;
    padding: 30px;
    max-width: 500px;
    width: 100%;
    position: relative;
    box-shadow: 0 25px 50px rgba(255, 105, 180, 0.3);
    border: 2px solid #ffb6c1;
    transform: scale(0.7);
    opacity: 0;
    transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.modal-content.show {
    transform: scale(1);
    opacity: 1;
}

.close-btn {
    position: absolute;
    top: 15px;
    right: 20px;
    background: none;
    border: none;
    font-size: 30px;
    color: #d63384;
    cursor: pointer;
    transition: color 0.3s ease;
}

.close-btn:hover {
    color: #ff69b4;
}

.modal-body {
    transition: all 0.3s ease;
}

.image-gallery {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin: 15px 0;
    flex-wrap: wrap;
}

.modal-image {
    width: 80px;
    height: 80px;
    object-fit: cover;
    border-radius: 15px;
    border: 2px solid #ffb6c1;
    box-shadow: 0 4px 15px rgba(255, 105, 180, 0.3);
    transition: all 0.3s ease;
    cursor: pointer;
}

.modal-image:hover {
    transform: scale(1.1);
    box-shadow: 0 6px 20px rgba(255, 105, 180, 0.5);
    border-color: #ff69b4;
}

.modal-title {
    font-family: 'Great Vibes', cursive;
    font-size: 2rem;
    color: #d63384;
    text-align: center;
    margin-bottom: 20px;
    transition: all 0.3s ease;
}

.modal-message {
    font-family: 'Poppins', sans-serif;
    font-size: 16px;
    line-height: 1.6;
    color: #555;
    text-align: center;
    margin-bottom: 20px;
    transition: all 0.3s ease;
}

.flower-decoration {
    text-align: center;
    font-size: 1.5rem;
    animation: bounce 2s infinite;
    transition: all 0.3s ease;
}

/* Animations */
@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes float {

    0%,
    100% {
        transform: translateY(0);
    }

    50% {
        transform: translateY(-10px);
    }
}

@keyframes pulse {

    0%,
    100% {
        opacity: 1;
    }

    50% {
        opacity: 0.7;
    }
}

@keyframes bounce {

    0%,
    20%,
    50%,
    80%,
    100% {
        transform: translateY(0);
    }

    40% {
        transform: translateY(-5px);
    }

    60% {
        transform: translateY(-3px);
    }
}

@keyframes magicalEntrance {
    0% {
        opacity: 0;
        transform: translate(-50%, -50%) scale(0.5) rotateZ(-180deg);
        filter: blur(10px);
    }
    50% {
        transform: translate(-50%, -50%) scale(1.15) rotateZ(-90deg);
        filter: blur(2px);
    }
    100% {
        opacity: 1;
        transform: translate(-50%, -50%) scale(1) rotateZ(0deg);
        filter: blur(0px);
    }
}

@keyframes layerShift {
    0%, 100% {
        transform: translateX(0) scale(1);
    }
    50% {
        transform: translateX(5px) scale(1.02);
    }
}

@keyframes iconFloat {
    0%, 100% {
        transform: translateY(0) rotate(0deg);
    }
    50% {
        transform: translateY(-2px) rotate(5deg);
    }
}

@keyframes heartPulse {
    0%, 100% {
        transform: scale(1);
        opacity: 0.8;
    }
    50% {
        transform: scale(1.2);
        opacity: 1;
    }
}

@keyframes textGlow {
    0%, 100% {
        text-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
    }
    50% {
        text-shadow: 0 2px 8px rgba(0, 0, 0, 0.3), 0 0 20px rgba(255, 255, 255, 0.3);
    }
}

@keyframes glowPulse {
    0%, 100% {
        opacity: 0.5;
        transform: translate(-50%, -50%) scale(1);
    }
    50% {
        opacity: 1;
        transform: translate(-50%, -50%) scale(1.1);
    }
}

@keyframes sparkleFloat {
    0%, 100% {
        transform: translateY(0) rotate(0deg);
        opacity: 0.7;
    }
    25% {
        transform: translateY(-5px) rotate(90deg);
        opacity: 1;
    }
    50% {
        transform: translateY(-2px) rotate(180deg);
        opacity: 0.8;
    }
    75% {
        transform: translateY(-7px) rotate(270deg);
        opacity: 1;
    }
}

@keyframes slideUp {
    0% {
        opacity: 0;
        transform: translateY(50px);
    }
    100% {
        opacity: 1;
        transform: translateY(0);
    }
}

/* Responsive Design */
@media (max-width: 768px) {
    .cover-container {
        padding: 15px;
    }

    .gift-image {
        width: 280px;
        height: 280px;
    }

    .bouquet-image {
        width: 280px;
        height: 350px;
    }

    .celebration-text h1 {
        font-size: 2rem;
        padding: 0 15px;
    }

    .celebration-subtitle {
        font-size: 14px;
        padding: 0 15px;
    }

    .love-letter-btn {
        width: 160px;
        height: 55px;
    }

    .btn-text {
        font-size: 13px;
    }

    .btn-icon {
        width: 18px;
        height: 18px;
    }

    .modal-content {
        margin: 15px;
        padding: 25px 20px;
        max-width: calc(100vw - 30px);
    }

    .modal-title {
        font-size: 1.6rem;
    }

    .modal-message {
        font-size: 15px;
        line-height: 1.5;
    }

    .modal-image {
        width: 70px;
        height: 70px;
    }

    .final-image {
        width: 280px;
        height: 280px;
    }

    .final-text h1 {
        font-size: 2.2rem;
        padding: 0 15px;
    }

    .final-subtitle {
        font-size: 1rem;
        padding: 0 15px;
    }
}

@media (max-width: 480px) {
    .cover-container {
        padding: 10px;
    }

    .gift-image {
        width: 250px;
        height: 250px;
    }

    .bouquet-image {
        width: 240px;
        height: 300px;
    }

    .celebration-text h1 {
        font-size: 1.7rem;
        padding: 0 10px;
    }

    .celebration-subtitle {
        font-size: 13px;
        padding: 0 10px;
    }

    .love-letter-btn {
        width: 140px;
        height: 50px;
    }

    .btn-text {
        font-size: 12px;
    }

    .btn-icon {
        width: 16px;
        height: 16px;
    }

    .heart-float {
        font-size: 10px;
    }

    .tap-text {
        font-size: 16px;
        padding: 0 10px;
    }

    .modal-content {
        margin: 10px;
        padding: 20px 15px;
        border-radius: 20px;
    }

    .modal-title {
        font-size: 1.4rem;
        margin-bottom: 15px;
    }

    .modal-message {
        font-size: 14px;
        line-height: 1.4;
        margin-bottom: 15px;
    }

    .close-btn {
        top: 10px;
        right: 15px;
        font-size: 26px;
    }

    .modal-image {
        width: 60px;
        height: 60px;
    }

    .image-gallery {
        gap: 8px;
        margin: 10px 0;
    }

    .final-image {
        width: 240px;
        height: 240px;
    }

    .final-text h1 {
        font-size: 1.8rem;
        padding: 0 10px;
    }

    .final-subtitle {
        font-size: 0.9rem;
        padding: 0 10px;
    }

    .final-container {
        gap: 20px;
    }
}

@media (max-width: 360px) {
    .gift-image {
        width: 220px;
        height: 220px;
    }

    .bouquet-image {
        width: 200px;
        height: 260px;
    }

    .celebration-text h1 {
        font-size: 1.5rem;
    }

    .love-letter-btn {
        width: 120px;
        height: 45px;
    }

    .btn-text {
        font-size: 11px;
    }

    .btn-icon {
        width: 14px;
        height: 14px;
    }

    .heart-float {
        font-size: 9px;
    }

    .modal-image {
        width: 50px;
        height: 50px;
    }

    .image-gallery {
        gap: 6px;
        margin: 8px 0;
    }

    .final-image {
        width: 200px;
        height: 200px;
    }

    .final-text h1 {
        font-size: 1.5rem;
    }

    .final-subtitle {
        font-size: 0.8rem;
    }

    .final-container {
        gap: 15px;
    }
}
</style>