<script setup>
import { ref, watchEffect } from 'vue';
import axios from 'axios';
import ProductCard from '@/components/ProductCard.vue';
import Pagination from '@/components/Pagination.vue';
import Loading from '@/components/Loading.vue';
import ProductForm from '@/components/ProductForm.vue';

const products = ref([]);
const page = ref(1);
const limit = ref(9);
const BASE_URL = `http://localhost:3000/products`
const isLoading = ref(true);


async function fetchData() {
	try {
		isLoading.value = true
		products.value = await axios
			.get(`${BASE_URL}?_page=${page.value}&_per_page=${limit.value}`)
			.then((res) => res.data);
	} catch (error) {
		console.log(error)
	} finally {
		isLoading.value = false
	}
}

watchEffect(() => {
	fetchData();
})

function changePage(newPage) {
	if (newPage < 1) return;
	if (newPage > products.value.pages) return;
	page.value = newPage;
}

async function createProduct(product) {
	try {
		await axios.post(BASE_URL, product);
		fetchData()
	} catch (error) {
		console.log("Terjadi kesalahan saat kirim product:", error)
	}
}
</script>

<template>
	<div v-if="isLoading">
		<Loading />
	</div>

	<main v-else>
		<ProductForm @create-product="createProduct" />
		<div class="product-grid">
			<!-- Kirim data ke ProductCard.vue menggunakan props -->
			<ProductCard v-for="product in products.data" :key="product.id" :product="product" />

		</div>
		<div class="pagination">
			<Pagination :page="page" :totalPages="products.pages" @change-page="changePage" />
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
