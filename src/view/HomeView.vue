<script setup>
import { reactive, ref } from 'vue';
import axios from 'axios';
import ProductCard from '@/components/ProductCard.vue';
import Pagination from '@/components/Pagination.vue';

const products = ref([]);
const page = ref(1)
const limit = ref(8)

products.value = await axios
	.get(`http://localhost:3000/products?_page=${page.value}&_per_page=${limit.value}`)
	.then((res) => res.data);

</script>

<template>
	<main>
		<div class="product-grid">
			<!-- Kirim data ke ProductCard.vue menggunakan props -->
            <ProductCard 
			v-for="product in products.data" 
			:key="product.id" 
			:product="product"
			/>
			
		</div>
		<div class="pagination">
			<Pagination/>
		</div>
	</main>
</template>

<style scoped>
.product-grid {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
	gap: 20px;
	width: 80%;
	margin: 0 auto;
}

.pagination {
	display: flex;
	justify-content: center;
	align-items: center;
	margin-top: 20px;
}
</style>