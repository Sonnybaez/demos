<!--
  CHRI-320: Post-Purchase Confirmation Clarity
  Annotated diff against the live PurchaseCompleted.vue
  
  This file shows ONLY the parts of the template that need to change.
  Changes are wrapped with <!-- CHANGED --> and <!-- /CHANGED --> comments
  for easy diffing. Additions are wrapped with <!-- ADDED -->.
  
  The <script setup>, <style scoped>, and untouched parts of the template
  are not reproduced here — refer to the original PurchaseCompleted.vue.
-->

<template>
  <BasicLayout class="PurchaseCompleted">
    <Head :title="steps[currentProgressIndex]?.[0]"/>

    <main>
      <RouterView v-slot="{ Component }">
        <component
          :is="Component"
          v-model="signUp"
          :user="user"
          :personaConfig="personaConfig"
          :userAttemptedNavigateForward="userAttemptedNavigateForward"
        >

          <!-- ============================================================ -->
          <!-- INTRO SLOT — copy changes for payment-led messaging          -->
          <!-- ============================================================ -->
          <template #intro>
            <div class="intro rc">

              <!-- CHANGED: header replaced
                   Was:  <h1>Almost there</h1>
                   Why:  "Almost there" doesn't confirm payment or create urgency.
                         The new header makes it explicit that money has changed
                         hands and verification is the only outstanding step. -->
              <h1>Payment received — let's get your order moving</h1>
              <!-- /CHANGED -->

              <!-- CHANGED: subhead replaced
                   Was:  <p>We've received your request, but we need to verify
                          your identity before we proceed with your prescription.</p>
                   Why:  "request" is wrong — they've placed and paid for an order.
                         New copy reinforces payment + names the order number +
                         tells the user exactly what unblocks dispatch. -->
              <p>We've received your payment for order <strong>CHEQ-{{ user.id }}</strong>. Complete the steps below and your prescription goes to a clinician for review.</p>
              <!-- /CHANGED -->

              <!-- CHANGED: lead-in line shortened
                   Was:  <p>We ask you to provide:</p>
                   Why:  "We just need:" is more conversational and matches
                         brand tone of voice principle 1 ("talk like a person"). -->
              <p>We just need:</p>
              <!-- /CHANGED -->

              <ul>
                <li>Verification of a valid photographic ID.</li>
                <li>A photo of you standing.</li>
                <li v-if="isHigherDose">Evidence of a previous prescription.</li>
              </ul>
            </div>
          </template>

          <!-- ============================================================ -->
          <!-- ORDER NUMBER + PROGRESS STEPS                                -->
          <!-- ============================================================ -->
          <div class="max-w-none flex flex-col sm:flex-row-reverse gap-6 items-start justify-between">

            <!-- CHANGED: OrderNumber label prop
                 Was:  <OrderNumber :orderNumber="`CHEQ-${user.id}`"/>
                 Why:  Adding a "label" prop so the badge can read "Paid order"
                       instead of "Order number". This requires a small tweak
                       to OrderNumber.vue to accept the new prop with a default
                       of "Order number" so existing callers don't break. -->
            <OrderNumber
              :orderNumber="`CHEQ-${user.id}`"
              label="Paid order"
            />
            <!-- /CHANGED -->

            <ProgressSteps
              :currentStepIndex="currentProgressIndex"
              :maximumStepIndex="currentProgressIndex"
              :stepNames="progressSteps"
            />
          </div>

          <!-- ADDED: dispatch timing reassurance
               Why:  45% of unrealised cancellations happen within 24 hours.
                     The 14:00 cutoff is currently only mentioned on the patient
                     dashboard, not on this page. Surfacing it here gives the
                     user an immediate reason to complete now rather than later.
               Note: this should only render Mon-Fri before 14:00 to be most
                     effective — see DispatchTimingNote.vue (new component
                     scoped in CHRI-320 implementation ticket). -->
          <DispatchTimingNote v-if="showDispatchTiming" />
          <!-- /ADDED -->

          <!-- ============================================================ -->
          <!-- REMAINING SLOTS — UNCHANGED                                  -->
          <!-- noCameraFAQ, privacyNote, buttonRow                          -->
          <!-- ============================================================ -->

          <template #noCameraFAQ>
            <h3 class="!text-potent-purple">What if you have no camera?</h3>
            <a class="qr no-a" href="https://start.chequp.com/patient">
              <img
                src="@/assets/images/qr-start-chequp-com-patient.webp"
                alt="QR code of the web address of the CheqUp account log-in page."
              >
            </a>
            <p>If the device you're using has no camera or you would prefer to complete these steps on another device, you can now log in to your account on another device by clicking or scanning the QR code.</p>
          </template>

          <template #privacyNote>
            <div class="privacy-note rc">
              <p v-if="routeEnd === 'inform-gp'">The information you provide will be transmitted securely to your GP practice. Please ensure all details are accurate to avoid delays.</p>
              <p>
                Your data is safe with CheqUp. We adhere to the highest online security standards and are recognised by the leading UK medical security standards. For more information, see our <a class="a" target="_blank" href="https://chequp.com/privacy-policy/">privacy policy</a>.
              </p>
            </div>
          </template>

          <template #buttonRow>
            <!-- unchanged — see original PurchaseCompleted.vue lines 377-418 -->
          </template>

        </component>
      </RouterView>
    </main>
  </BasicLayout>
</template>

<!--
  ============================================================
  SUMMARY OF CHANGES
  ============================================================
  
  1. h1 copy:           "Almost there"
                        → "Payment received — let's get your order moving"
  
  2. Subhead p copy:    "We've received your request, but we need to verify
                         your identity before we proceed with your prescription."
                        → "We've received your payment for order CHEQ-{{user.id}}.
                           Complete the steps below and your prescription goes
                           to a clinician for review."
  
  3. List lead-in:      "We ask you to provide:"
                        → "We just need:"
  
  4. OrderNumber:       Add `label="Paid order"` prop
                        Requires OrderNumber.vue to accept a `label` prop
                        with default "Order number"
  
  5. New element:       <DispatchTimingNote /> below the OrderNumber/Progress row
                        New component, see scope notes in CHRI-320
  
  ============================================================
  COMPONENTS THAT NEED UPDATING
  ============================================================
  
  - PurchaseCompleted.vue   (this file — 5 changes above)
  - OrderNumber.vue         (add `label` prop with default)
  - DispatchTimingNote.vue  (new component, scoped in CHRI-320)
  
  ============================================================
  NO BACKEND CHANGES REQUIRED
  ============================================================
  
  All changes are template/copy only. No new fields, no API
  changes, no database migrations.
-->
