<template>
    <div class="fixed z-[1] w-full h-full overflow-auto bg-[rgba(0,0,0,0.4)] xl:py-[50px] p-4 left-0 top-0"
        @click.self="closeModel">

        <!-- Modal content -->
        <div class="container mx-auto bg-white xl:overflow-hidden overflow-auto rounded min-h-full h-full relative py-3">
            <div class="h-fit w-fit top-0 right-2 cursor-pointer font-bold absolute xl:block hidden p-3"
                @click="closeModel">
                &#x2715;
            </div>

            <!-- Desktop view -->
            <div class="w-[97%] h-full mx-auto xl:flex flex-col hidden">
                <ul class="flex border-b border-black pb-1">
                    <li class="font-semibold uppercase text-sm mx-2 py-2 cursor-pointer"
                        :class="{ 'border-b-[3px] border-[#6082B6]': activeTab === 0, 'opacity-70': activeTab !== 0 }" @click="firstVideoIndex()">video
                    </li>
                    <li class="font-semibold uppercase text-sm mx-2 py-2 cursor-pointer"
                        :class="{ 'border-b-[3px] border-[#6082B6]': activeTab === 1, 'opacity-70': activeTab !== 1 }" @click="firstImageIndex()">images
                    </li>
                </ul>

                <div class="relative flex mt-1 max-h-full pt-4 pb-16">
                    <!-- Image div -->
                    <div class="p-2 grow h-full overflow-hidden mx-auto  border-2 " ref="containerDiv"
                        @click="toggleImageClick"
                        @mousemove="handleMouseMove" @mouseleave="resetZoom"
                        :class="{ 'cursor-zoom-out': activeTab === 1 && isMouseClickOnImage, 'cursor-zoom-in': activeTab === 1 && !isMouseClickOnImage }">
                        <img :src="details.thumbnails[currentIndex].img" ref="img"
                            v-if="details.thumbnails[currentIndex].img && activeTab === 1"
                            class="mx-auto h-full" alt="thumb.img">
                        <video class="h-full w-full p-2" controls autoplay
                            v-else-if="details.thumbnails[currentIndex].video && activeTab === 0">
                            <source :src="details.thumbnails[currentIndex].video" type="video/mp4">
                        </video>
                    </div>

                    <!-- Details div -->
                    <div class="w-96 pl-4 bg-slate-50">
                        <!-- Product name -->
                        <h1 class="text-[24px] font-semibold leading-none capitalize">{{ details.name }}</h1>
                        
                        <p class="md:text-sm opacity-70 text-base font-semibold py-10 text-[15px]">
                            The Samsung Samsung Galaxy S23+ 5G Dual SIM Android Mobile Phone, 512GB (UK Version) is a flagship smartphone that belongs to the Samsung series, offering users a premium experience with cutting-edge technology and a sleek design.
                        </p>
                        
                        <div class="grid grid-cols-4 gap-4 mt-4">
                            <!-- For image tab -->
                            <template v-for="(thumb, ind) in details.thumbnails" :key="ind">
                                <div @click="setCurrentIndex(ind)" v-if="activeTab === 1 && thumb && thumb.img"
                                    class="border-2 rounded p-2 cursor-pointer"
                                    :class="{ 'border-[#6082B6]': ind === currentIndex }">
                                    <img :src="thumb.img" v-if="thumb.img && thumb" class="aspect-square object-contain"
                                        style="transition: transform 0.5;" alt="thumb.img">
                                </div>
                                <div class="aspect-square border-2 rounded p-2 cursor-pointer"
                                    :class="{ 'border-[#6082B6]': ind === currentIndex }"
                                    v-else-if="thumb.video && thumb && activeTab === 0">
                                    <span>
                                        <img src="../assets/images/videoThumbnail.png" class="xl:w-16 w-8 aspect-square" alt="video-thumbnail">
                                    </span>
                                </div>
                            </template>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Mobile view -->
            <div class="h-full flex flex-col xl:hidden min-h-fit">
                <button class="shadow border py-1 px-4 w-fit font-bold rounded ml-2 "
                    @click="closeModel">Back</button>
                <div class="grow pt-1">
                    <div class="h-full max-h-[90%] mx-1 relative overflow-hidden p-1`" ref="containerMobile" @click="toggleMobileZoom">
                        <img :src="details.thumbnails[currentIndex].img" v-if="details.thumbnails[currentIndex].img"
                            class="h-auto mx-auto object-contain aspect-square pt-10" ref="imgMobile"
                            alt="details.thumbnails[currentIndex].img" style="transition: transform 0.5s ease;">
                        <video class="h-full w-full py-2" controls autoplay
                            v-else-if="details.thumbnails[currentIndex].video">
                            <source :src="details.thumbnails[currentIndex].video" type="video/mp4">
                        </video>
                    </div>

                    <div class="overflow-y-auto w-full px-2 pt-0">
                        <div class="flex h-16 sm:h-20 flex-nowrap">
                            <div v-for="(thumb, ind) in details.thumbnails" :key="index"
                                class="aspect-square w-16 sm:w-20 p-1 grid place-items-center rounded"
                                :class="{ 'border-2 border-[#6082B6]': currentIndex === ind }" @click="setCurrentIndex(ind)">
                                <img :src="thumb.img" v-if="thumb.img" class="aspect-square object-contain" alt="thumb.img">
                                <span v-if="thumb.video">
                                    <img src="../assets/images/videoThumbnail.png" class="xl:w-16 w-8 aspect-square" alt="Left-btn">
                                </span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        index: {
            default: 0
        },
        type: {
            default: 1
        },
        details: {
            required: true
        }
    },
    data() {
        return {
            activeTab: this.type,
            currentIndex: this.index,
            dblTapImageZoom: true,
            isMouseClickOnImage: false,
            isMouseClickOnImageMobile: false,
            zoomFactor: 2,
            zoomFactorMobile: 2
        };
    },
    methods: {
        closeModel() {
            this.$emit('closeModel');
            this.isMouseClickOnImage = false;
            this.isMouseClickOnImageMobile = false;
        },
        firstVideoIndex() {
            this.activeTab = 0;
            for (let i = 0; i < this.details.thumbnails.length; i++) {
                if (this.details.thumbnails[i].                    video) {
                    this.currentIndex = i;
                    break;
                }
            }
        },
        firstImageIndex() {
            this.activeTab = 1;
            for (let i = 0; i < this.details.thumbnails.length; i++) {
                if (this.details.thumbnails[i].img) {
                    this.currentIndex = i;
                    break;
                }
            }
        },
        handleMouseMove(e) {
            const { left, top, width, height, right, bottom } = this.$refs.containerDiv.getBoundingClientRect();

            const x = e.clientX - (right - (left + 300));
            const y = e.clientY - (bottom - (top + 200));

            const xPercent = (x / width) * 100;
            const yPercent = (y / height) * 100;

            const offsetX = ((xPercent / 100) * (this.zoomFactor - 1)) * width;
            const offsetY = ((yPercent / 100) * (this.zoomFactor - 1)) * width;

            if (this.isMouseClickOnImage) {
                this.$refs.img.style.transform = `translate(${-offsetX}px, ${-offsetY}px) scale(${this.zoomFactor})`;
            }
        },
        toggleImageClick() {
            this.isMouseClickOnImage = !this.isMouseClickOnImage;
            this.resetZoom();
        },
        toggleMobileZoom(event) {
            this.dblTapImageZoom = !this.dblTapImageZoom;
            if (this.dblTapImageZoom) {
                this.isMouseClickOnImageMobile ? this.resetMobileZoom() : this.zoomMobileImg(event);
                this.isMouseClickOnImageMobile = !this.isMouseClickOnImageMobile;
            }
        },
        resetZoom() {
            this.$refs.img.style.transform = 'none';
        },
        resetMobileZoom() {
            this.$refs.imgMobile.style.transform = "none";
        },
        zoomMobileImg(e) {
            const transformVal = this.$refs.imgMobile.style.transform;

            if (transformVal) {
                const match = transformVal.match(/scale\((\d+(\.\d+)?)\)/);

                if (match && match[1]) {
                    this.zoomFactorMobile = parseFloat(match[1]);
                }
            }

            this.$refs.imgMobile.style.transform = 'scale(' + this.zoomFactorMobile + ')';

            const boundingRect = this.$refs.imgMobile.getBoundingClientRect();
            const x = e.clientX - boundingRect.left;
            const y = e.clientY - boundingRect.top;

            this.$refs.imgMobile.style.transformOrigin = (x / boundingRect.width) * 100 + '% ' + (y / boundingRect.height) * 100 + '%';
        },
        setCurrentIndex(ind) {
            this.currentIndex = ind;
        }
    }
};
</script>

