<template>
    <ion-menu class="AppMenu" side="end" content-id="main-content">
      <ion-header>
        <ion-toolbar>
          <ion-title>Menu Content</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-content>
        <ion-list>
          <ion-item @click="closeMenu">
            <ion-button size="default" router-link="/users" expand="full">Managing Users</ion-button>
          </ion-item>
          <ion-item v-if="isNat" @click="closeMenu">
            <ion-button size="default" router-link="/passphrase" expand="full">{{ textPass }}</ion-button>
          </ion-item>
          <ion-item v-if="isNat && isPassSet" @click="closeMenu">
            <ion-button size="default" router-link="/encryption" expand="full">DB's Encryption</ion-button>
          </ion-item>
          <!-- ... other menu items -->
        </ion-list>
      </ion-content>
    </ion-menu>
</template>
  
<script lang="ts">
import { IonMenu, IonHeader, IonToolbar, IonTitle, IonContent, IonList,
        IonItem, IonButton } from '@ionic/vue';
import { defineComponent, getCurrentInstance, ref, watch } from 'vue';
import { useIonRouter } from '@ionic/vue';

export default defineComponent({
    name: 'AppMenu',
    components: { IonMenu, IonHeader, IonToolbar, IonTitle, IonContent,
                IonList, IonItem, IonButton },
    setup() {
        const router = useIonRouter();
        const appInstance = getCurrentInstance()!;
        const globalState = appInstance.appContext.config.globalProperties.$globalState;
        const isPassphrase: boolean = globalState.isPassphrase;
        const isNative: boolean = globalState.isNative;
        const isPassSet = ref(isPassphrase);
        const isNat = ref(isNative);
        const textPass = ref(isPassphrase ? "Change Passphrase" : "Set Passphrase")

        const closeMenu = () => {
            const menu = document.querySelector('ion-menu');
            menu!.close();
        };
        watch( () => globalState.isPassphrase,
            (newValue, oldValue) => {
                // React to changes in isDisplay here, if needed
                isPassSet.value = newValue;
                textPass.value = newValue ? "Change Passphrase" : "Set Passphrase"; 
            }
        );
        return {closeMenu, router, isPassSet, textPass, isNat}
    },
});
</script>
<style scoped>
    .AppMenu  ion-button {
        --background: transparent;
        --color: initial;
        font-size: 18px;
    }
</style>
  