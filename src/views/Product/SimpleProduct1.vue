<script setup lang="ts">
import { useProduct } from '@/stores/product';
import { computed } from 'vue';
import { useRouter } from 'vue-router'; 
import ProductVariation from '@/components/01/SimpleProduct/ProductVariation.vue';
import Gallery from '@/components/01/Product/Gallery.vue';
import AddToCard from '@/components/01/SimpleProduct/AddToCart.vue';

const productStore = useProduct();

const productData = computed(() => productStore.data);

const router = useRouter(); 
const handleVariationChange = async (slug: string) => {
  await productStore.getProduct(slug);  
  router.push({ name: 'Product', params: { slug } }); 
};

</script>

<template>
  <div>
    <div class="container mt-lg-5 pt-lg-5 px-lg-1">
      <div class="row mb-5">
        <div class="col-xl-6 pe-md-5">
          <Gallery :productData="productData" />
        </div>
        <div class="col-xl-6">
          <div class="bg-light p-1 p-md-5">
            <div class="sticky_summary_details mobile pt-5">
              <div class="d-flex justify-content-between">
                <div class="flex-grow-1 ms-3">
                  <h1 class="fw-bold">{{ productData.name }}</h1>
                  <div class="d-flex flex-row align-items-center gap-2 price" style="font-size:16px;">
                    <span>Price:</span>
                    <span class="pricecontent h2 fw-bold text-secondary m-0" style="font-size:18px;">
                      <span class="price_span" style="font-size:24px;">{{ productData.price }}</span>
                    </span>
                    <span class="if-discounted" style="display: none">$<span id="dis_price">0.00</span></span>
                  </div>
                  <ProductVariation v-if="productData.variations !== ''" :attribute="productData.variations" @variationChange="handleVariationChange" />
                  <div class="mt-5" v-html="productData.description" ref="input"></div>
                </div>
              </div>
            </div>
          </div>
          <AddToCard :product="productData"/>
        </div>
      </div>
    </div>
  </div>
</template>


