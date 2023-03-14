<template>
    <section class="bg-gray-100 bg-opacity-50 text-gray-800">
        <div class="py-10 px-10">
            <div class="flex flex-col lg:flex-row gap-8">
                <div class="w-full lg:w-4/6 py-10 font-semibold text-lg">
                    <h1 class="font-extrabold text-center text-4xl">Features of Imegia</h1>
                    <div class="feature-section">
                        <div class="feature bg-orange-100">
                            <img src="src\assets\images\refusing.png" class="w-20 h-20" alt=""/>
                            <h6 class="">Free To Use</h6>
                        </div>
                        <div class="feature">
                            <img src="src\assets\images\running-rabbit.png" class="w-20 h-20" alt=""/>
                            <h6 class="">Super Fast API</h6>
                        </div>
                        <div class="feature">
                            <img src="src\assets\images\infinity.png" class="w-20 h-20" alt="" />
                            <h6 class="">No Limit</h6>
                        </div>
                        <div class="feature">
                            <img src="src\assets\images\gallery.png" class="w-20 h-20" alt="tht" />
                            <h6 class="">Supports All File Types</h6>
                        </div>
                    </div>

                </div>

                <div class="w-full lg:w-1/3 flex flex-col items-center gap-10 py-10">
                    <h1 class="font-extrabold text-center px-8 pt-8 text-2xl">Are you looking for more APIs?<br><br>Check all APIs
                        made by us!</h1>
                    <a href="https://oyyi.xyz/"><h4
                        class="btn-visit-oyyi">
                        Visit Oyyi <img alt="title" src="https://img.icons8.com/small/16/FFFFFF/external-link.png"
                            class="w-4 h-4" /></h4></a>
                </div>
            </div>
        </div>

    </section>
</template>
<style scoped>
.feature-section {
    @apply flex flex-wrap py-10 gap-x-14 gap-y-8 justify-center
}

.feature {
    @apply flex items-center gap-8 bg-orange-100 bg-opacity-50 border-2 border-orange-100 rounded-2xl py-2 px-6 w-full md:w-2/5
}

.btn-visit-oyyi {
    @apply flex justify-center gap-x-2 bg-orange-400 hover:bg-orange-500 text-white font-bold py-2 px-4 rounded-full w-36
}
</style>