<template>
    <BModal
        class="consent-form-modal"
        id="bv-modal"
        size="lg"
        centered
        title="Port of Mars Consent Form"
        no-stacking
        :header-bg-variant="headerBgVariant"
        :header-text-variant="headerTextVariant"
        :body-bg-variant="bodyBgVariant"
        :body-text-variant="bodyTextVariant"
        :footer-bg-variant="footerBgVariant"
        :footer-text-variant="footerTextVariant"
        :no-close-on-backdrop="true"
        :no-close-on-esc="true"
        :hide-header-close="true"
    >
        <template v-slot:modal-title>
            <code><b>CONSENT FORM</b></code>
        </template>
        <div class="d-block text-center">
            <p align="justify">Dear Participant,</p>

            <p align="justify">
                I am a professor in the School of Sustainability at Arizona State University.
                I am conducting experiments that investigate how people think, act, and make decisions.
                This research is part of the Interplanetary Research Initiative at Arizona State University.
            </p>

            <p align="justify">
                I am requesting your participation, which will involve participating in a game tournament.
                The game will take a maximum of 60 minutes. The tournament has 4 rounds to determine the Mars Madness
                champion, and if you qualify for another round, such a game will take another hour at a later day with a
                special invitation. Your participation in this study is voluntary. <code><b>You must be 18 or older to participate
                in the study.</b></code> If you choose not to participate or to withdraw from the study at any time, there will be
                no penalty; it will not affect your compensation for participation up to that point.
            </p>

            <p align="justify">
                During the game you can chat with other participants. By signing this consent form, you consent to:
                <ul>
                    <li>Abstain from personal attacks or harassment</li>
                    <li>Will not use profanity or offensive language</li>
                </ul>
            </p>

            <p align="justify">
                For participation in this study you may receive extra credit if your instructor of one of the
                participating classes has made the participation in this event an extra credit opportunity. Those
                who qualify for the championship round will be invited to have lunch with astronaut in residence,
                Catherine Coleman. The winner of the Mars Madness championship round will be able to create a new
                event for the next edition of Mars Madness.
            </p>

            <p align="justify">
                Society may benefit from this research because an understanding of how people make decisions can
                help us to design regulations that sustain the use of shared resources, in this experiment in a
                colony on Mars.  You may benefit from this experience because you learn something about how an
                experiment is designed and conducted, what issues are of interest to social scientists and space
                research, and how your own cognitive abilities come into play in decision making situations.
                There are no foreseeable risks or discomforts to your participation.
            </p>

            <p align="justify">
                The results of the research study may be published, but your name will not be used. Your
                responses will be confidential. However, due to the group nature of this study, complete
                confidentiality cannot be guaranteed. We contact your instructor that you have participated,
                if your instructor asked us to do so for an extra credit assignment. We also keep track who
                moves to next rounds. We delete personal information such as your email address from our database
                after the tournament is completed.
            </p>

            <p align="justify">
                If you have any questions concerning the research study, contact me at
                <a href="mailto:Marco.Janssen@asu.edu">Marco.Janssen@asu.edu</a>.
            </p>

            <p align="justify">Sincerely,</p>
            <p align="justify">Dr. Marco Janssen</p>

            <code><b>By signing with an email that you check regularly and clicking below, you are giving consent to participate in the above study.</b></code>

            <br/><br/>

            <BContainer fluid>
                <BRow class="my-1">
                    <BIcon class="h4 my-2" icon="envelope-fill" lg />
                    <BCol>
                        <div role="group">
                            <BFormInput
                                id="input-live"
                                v-model="email"
                                :state="emailState()"
                                aria-describedby="input-live-help input-live-feedback"
                                placeholder="me@example.com"
                                type="email"
                                required
                                trim
                            />
                            <BFormInvalidFeedback id="input-live-feedback">
                                Enter an email in the correct format.
                            </BFormInvalidFeedback>
                            <BFormText id="input-live-help">Your email.</BFormText>
                        </div>
                    </BCol>

                </BRow>
            </BContainer>

            <br/>

            <div>
                <BButton squared class="button-consent" variant="success" :disabled="!emailState()" @click="grantConsent">Grant Consent</BButton>
                <BButton squared class="button-consent" variant="danger" @click="denyConsent">Deny Consent</BButton>
            </div>
        </div>
        <template v-slot:modal-footer>
            <p align="justify">This research has been reviewed and approved by the Social Behavioral IRB. You may talk to them at
               <b>(480) 965-6788</b> or by email at <a href="mailto:research.integrity@asu.edu">research.integrity@asu.edu</a> if:
               <br/><br/>
               <ul>
                   <li>Your questions, concerns, or complaints are not being answered by the research team.</li>
                   <li>You cannot reach the research team.</li>
                   <li>You want to talk to someone besides the research team.</li>
                   <li>You have questions about your rights as a research participant.</li>
                   <li>You want to get information or provide input about this research.</li>
               </ul>
            </p>
        </template>
    </BModal>
</template>

<script lang="ts">
import { Component, Vue, Emit } from 'vue-property-decorator';
import { url } from '@port-of-mars/client/util';
import _ from 'lodash';
import { BModal, BButton, BootstrapVueIcons,
         BFormInput, BContainer, BRow, BCol,
         BFormInvalidFeedback, BFormText } from 'bootstrap-vue';
        

Vue.use(BootstrapVueIcons)

@Component({
    components: {
        BContainer,
        BRow,
        BCol,
        BModal,
        BButton,
        BFormInput,
        BFormInvalidFeedback,
        BFormText
    }
})

export default class ConsentFormModal extends Vue {
    variants = [
        'primary',
        'secondary',
        'success',
        'warning',
        'danger',
        'info',
        'light',
        'dark'
    ];

    headerBgVariant: string = 'dark';
    headerTextVariant: string = 'light';
    bodyBgVariant: string = 'dark';
    bodyTextVariant: string = 'light';
    footerBgVariant: string = 'danger';
    footerTextVariant: string = 'light';

    email: string = '';

    emailState() {
      return !_.isEmpty(this.email);
    }

    async grantConsent() {
      this.$bvModal.hide('bv-modal');
      // FIXME: post to a new endpoint to store the user's consent and timestamp
      await this.$ajax.post(url('/registration/tutorial'), ({data, status}) => {
        // what should we do after this post? return to the dashboard after modifying consent granted
        // FIXME: run an api call to commit to the store instead of modifying state directly
        this.$tstore.commit('SET_CONSENT', true);
      });
    }

    denyConsent() {
      this.$bvModal.hide('bv-modal');
      this.$tstore.commit('SET_CONSENT', false);
    }

}

</script>

<style lang="scss" scoped>
@import '@port-of-mars/client/stylesheets/tutorial/ConsentFormModal.scss';
</style>
