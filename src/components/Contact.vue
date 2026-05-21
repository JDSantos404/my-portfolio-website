<script setup>
    import { ref, onMounted, onBeforeMount } from 'vue';

    import { Notyf } from 'notyf';
    import 'notyf/notyf.min.css';

    const notyf = new Notyf();


    const WEB3FORMS_ACCESS_KEY = "3ef59ef3-5d65-4167-aa9d-d67d2aad276f";

    const subject = ref("");
    const name = ref("");
    const email = ref("");
    const phone = ref("");
    const message = ref("");

    const isLoading = ref(false);


    const submitForm = async () => {
        isLoading.value = true;

        try {
            const response = await fetch("https://api.web3forms.com/submit", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    Accept: "application/json"
                },
                body: JSON.stringify({
                    access_key: WEB3FORMS_ACCESS_KEY,
                    subject: subject.value,
                    name: name.value,
                    email: email.value,
                    phone: phone.value,
                    message: message.value
                })
            });

            const result = await response.json();

            if (result.success) {
                console.log(result);
                notyf.success("Message sent!");
            }

        } catch (error) {
            console.log(error);
            notyf.error("Failed to send message");
        } finally {
            isLoading.value = false;
            resetRecaptcha();
        }
    };

    /*recaptcha integration*/

    const SITE_KEY ='6Lf3L_UsAAAAAHabBrZYRj73SHPcMXu-v7WN5UsA';

    const recaptchaContainer = ref(null);
    const recaptchaWidgetId = ref(null);
    const recaptchaToken = ref('');
    const captchaSize = window.innerWidth < 576
    ? 'compact'
    : 'normal';


    function onRecaptchaSuccess(token) {
        recaptchaToken.value = token;
    }

    function onRecaptchaExpired() {
        recaptchaToken.value = '';
    }

    function renderRecaptcha() {
        if(!window.grecaptcha) {
            console.error('reCAPTCHA not loaded');
            return;
        }

        recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
            sitekey: SITE_KEY,
            theme: 'dark',
            size: captchaSize,
            callback: onRecaptchaSuccess,
            'expired-callback': onRecaptchaExpired
        });
    }

    function resetRecaptcha() {
        if(recaptchaWidgetId.value !== null) {
            window.grecaptcha.reset(recaptchaWidgetId.value);
            recaptchaToken.value = '';
        }
    }

    onMounted(() => {
        const interval = setInterval(() => {
            if(window.grecaptcha && window.grecaptcha.render) {
                renderRecaptcha();
                clearInterval(interval)
            }
        }, 100);

        onBeforeMount(() => {
            clearInterval(interval);
        });
    })

</script>


<template>
        <!-- contact start -->
        <section class="section contact" id="contact">
            <div class="container">
                <div class="row g-5 align-items-start">

                    <!-- left side -->
                    <div class="col-lg-5">
                        <h2 class="section-title">Let's Collaborate</h2>
                        <p class="contact-subtitle">Have a project in mind? I'm open to freelance work, contract projects, and long-term collaborations. Let's build something great together.</p>

                        <div class="contact-info-list">
                            <div class="contact-info-item">
                                <i class="bi bi-envelope-fill text-teal"></i>
                                <span>jesusarcasdelossantosjr@gmail.com</span>
                            </div>
                            <div class="contact-info-item">
                                <i class="bi bi-telephone-fill text-teal"></i>
                                <span>(+63) 915 691 1843</span>
                            </div>
                            <div class="contact-info-item">
                                <i class="bi bi-geo-alt-fill text-teal"></i>
                                <span>Quezon City, Philippines</span>
                            </div>
                            <div class="contact-info-item">
                                <i class="bi bi-linkedin text-teal"></i>
                                <a href="https://www.linkedin.com/in/jssdlssnts/" target="_blank" rel="noopener noreferrer">linkedin.com/in/jssdlssnts</a>
                            </div>
                        </div>
                    </div>

                    <!-- right side -->
                    <div class="col-lg-7">
                        <form @submit.prevent="submitForm" class="contact-form">

                            <div class="row g-3">
                                <div class="col-sm-6">
                                    <label for="contact-name" class="form-label">Name</label>
                                    <input type="text" id="contact-name" 
                                    v-model="name"
                                    class="form-control contact-input" required autocomplete>
                                </div>
                                <div class="col-sm-6">
                                    <label for="contact-email" class="form-label">Email</label>
                                    <input type="email" id="contact-email"  
                                    v-model="email"
                                    class="form-control contact-input" required autocomplete>
                                </div>
                                <div class="col-sm-6">
                                    <label for="contact-number" class="form-label">Phone</label>
                                    <input type="number" id="contact-number"  
                                    v-model="phone"
                                    class="form-control contact-input" required autocomplete>
                                </div>
                                <div class="col-sm-6">
                                    <label for="contact-subject" class="form-label">Subject</label>
                                    <input type="text" id="contact-subject"  
                                    v-model="subject"
                                    class="form-control contact-input" required autocomplete>
                                </div>
                                <div class="col-12">
                                    <label for="contact-message" class="form-label">Message</label>
                                    <textarea id="contact-message" 
                                    v-model="message"
                                    class="form-control contact-input contact-textarea" required></textarea>
                                </div>
                                <button
                                  type="submit"
                                  class="btn btn-teal btn-lg contact-submit"
                                  :disabled="!recaptchaToken || isLoading"
                                >
                                  {{ isLoading ? "Sending..." : "Send Message" }}
                                </button>
                            </div>

                            <div class="recaptcha-wrapper mt-3">
                                <div ref="recaptchaContainer"></div>
                            </div>

                        </form>
                    </div>

                </div>
            </div>
        </section>    
        <!-- contact start -->
</template>