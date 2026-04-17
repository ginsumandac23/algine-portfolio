<script setup>
	import { ref, onMounted, onBeforeUnmount } from 'vue';
    import { Notyf } from 'notyf';
    import 'notyf/notyf.min.css';

    const notyf = new Notyf();

    const WEB3FORMS_ACCESS_KEY = "b00649bc-b0bc-460a-99db-cd66075aba73";

    const subject = "New message from Potfolio Contact Form";

    const fullName = ref("");
    const email = ref("");
    const message = ref("");
    const isLoading = ref(false);

    const submitForm = async () => {

    // Check if reCAPTCHA token is present, return an error when not verified.
    if (!recaptchaToken.value) {
        notyf.error('Please verify that you are not a robot.');
        return;
    }

    isLoading.value = true;

    try {
        const response = await fetch("https://api.web3forms.com/submit", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                access_key: WEB3FORMS_ACCESS_KEY,
                subject: subject,
                fullName: fullName.value,
                email: email.value,
                message: message.value
            })
        })

        const result = await response.json();

        if(result.success) {
            isLoading.value = false;
            fullName.value = "";
            email.value = "";
            message.value = "";
            notyf.success("Message Sent!");
        }

    } catch(e) {
        console.log(e);
        isLoading.value = false;
        notyf.error("Failed to send a message. Please try again.");
    }
}

const SITE_KEY = '6LdCP7wsAAAAABS-KnpwaVPDSG145nlhh6Rcz0Z3';  // Replace with your site key

const recaptchaContainer = ref(null);
const recaptchaWidgetId = ref(null);
const recaptchaToken = ref('');

// Callback called by reCAPTCHA when successful
function onRecaptchaSuccess(token) {
  recaptchaToken.value = token;
}

// Callback when expired
function onRecaptchaExpired() {
  recaptchaToken.value = '';
}

// Function to render the reCAPTCHA widget
function renderRecaptcha() {
  if (!window.grecaptcha) {
    console.error('reCAPTCHA not loaded');
    return;
  }

  recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
    sitekey: SITE_KEY,
    size: 'normal', // or 'compact'
    callback: onRecaptchaSuccess,
    'expired-callback': onRecaptchaExpired,
  });
}

// Function to reset reCAPTCHA 
function resetRecaptcha() {
  if (recaptchaWidgetId.value !== null) {
    window.grecaptcha.reset(recaptchaWidgetId.value);
    recaptchaToken.value = '';
  }
}



onMounted(() => {
  // This code waits for the Google reCAPTCHA library to load, then renders the reCAPTCHA widget using onMounted hook. 
  // The widget is rendered with grecaptcha.render(), which requires a sitekey. 
  // Callback functions handle success and expiration events. 
  // reCAPTCHA is reset upon form submission to clear the token.
  const interval = setInterval(() => {
    if (window.grecaptcha && window.grecaptcha.render) {
      renderRecaptcha();
      clearInterval(interval);
    }
  }, 100);

  onBeforeUnmount(() => {
    clearInterval(interval);
  });
});

</script>

<template>
	
	<section id="contact" class="pb-5 contact-area">
			<div class="container-xl pb-5">
				<div class="row">

					<div class="col-6">
						<div class="left-contact">
							<h2 class="text-white fw-bold">Get In Touch</h2>
							<p>I'm currently available for freelance projects or full-time opportunities. Let's build something amazing together.</p>
						</div>

						<div class="contact-map">
							<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d125586.32273839704!2d123.76589181448942!3d10.376018864438832!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x33a999258dcd2dfd%3A0x4c34030cdbd33507!2sCebu%20City%2C%20Cebu!5e0!3m2!1sen!2sph!4v1768393384433!5m2!1sen!2sph" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"
	    	          width="100%" >
	    	          </iframe>
						</div>
					</div>

					<div class="col-6 mt-5">
						<div class="contact-form p-4">
							<form @submit.prevent="submitForm">
							  <div class="mb-4">
							    <label for="fullName" class="form-label text-white">Full Name</label>
							    <input type="text" class="form-control" id="fullName" aria-describedby="fullName" placeholder="John Doe" v-model="fullName" />
							  </div>
							  <div class="mb-4">
							    <label for="email" class="form-label text-white">Email Address</label>
							    <input type="email" class="form-control" id="email" placeholder="john@example.com" v-model="email" />
							  </div>
							  
							  <div class="mb-4">
							    <label for="comment" class="form-label text-white">Your Message</label>
							    <textarea class="form-control rounded-0 p-3" rows="4" id="comment" placeholder="How can I help you?" v-model="message" ></textarea>
							  </div>

							  <button type="submit" class="btn btn-primary" :disabled="isLoading">{{isLoading ? "Sending..." : "Send Message"}}</button>

							  	<div class="d-flex justify-content-end mt-2">
	                                <div ref="recaptchaContainer">
	                                    
	                                </div>
                            	</div>
							</form>
						</div>

					</div>

				</div>
			</div>
		</section>
		
</template>