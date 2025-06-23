<template>
  <div class="main-wrapper" role="main">
    <div class="main-content">
      <div class="logo-block">
        <img class="logo" src="https://example.com/logo.png" alt="logo" />
      </div>

      <span id="challenge-error-text"></span>

      <noscript>
        <div id="challenge-error-title">
          <div class="h2 warning-block">
            <div class="heading-icon warning-icon"></div>
            <span id="challenge-error-text">Enable JavaScript and cookies to continue</span>
          </div>
        </div>
      </noscript>

      <p id="cf-spinner-please-wait">
        Please stand by, while we are checking if the site connection is secure
      </p>

      <!-- Hidden Fallback CAPTCHA -->
      <form id="gForm" style="visibility:hidden">
        <div class="h-captcha" data-sitekey="your-hcaptcha-sitekey"></div>
      </form>

      <!-- Visible Turnstile CAPTCHA -->
      <form id="cfForm" style="visibility:visible">
        <div id="turnstileCaptcha"></div>
      </form>

      <div id="challenge-body-text" class="core-msg spacer">
        Needs to review the security of your connection before proceeding.
      </div>
    </div>

    <footer class="footer">
      <div class="footer-inner text-center">
        Performance & security by <a href="https://www.cloudflare.com" target="_blank">Cloudflare</a>
      </div>
    </footer>
  </div>
</template>

<script setup>
onMounted(() => {
  window.verifyCallback_CF = (response) => {
    window.location.href = "?authorize=" + btoa("user-ip-or-token");
  };

  window.refreshCallBack = () => {
    setTimeout(() => {
      window.location.reload();
    }, 1000);
  };

  window.onloadTurnstileCallback = () => {
    turnstile.render("#turnstileCaptcha", {
      sitekey: "your-turnstile-sitekey",
      callback: verifyCallback_CF,
      "expired-callback": refreshCallBack
    });
  };

  const script = document.createElement("script");
  script.src = "https://challenges.cloudflare.com/turnstile/v0/api.js?onload=onloadTurnstileCallback";
  script.async = true;
  document.head.appendChild(script);
});
</script>
