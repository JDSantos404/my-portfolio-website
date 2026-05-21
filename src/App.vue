<script setup>
	import Header from './components/Header.vue';
	import Navbar from './components/Navbar.vue';
	import Landing from './components/Landing.vue';
	import Projects from './components/Projects.vue';
	import Tools from './components/Tools.vue';
	import Contact from './components/Contact.vue';
	import Footer from './components/Footer.vue';
</script>

<template>
  <Header />
  <Navbar />
  <main class="main-content">
  		<Landing />
  		<Projects />
  		<Tools />
  		<Contact />
  </main>
  <Footer />


</template>
