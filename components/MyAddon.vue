<script>
import Tool from '../../../src/modules/tools/Tool.vue';
import { mapActions, mapGetters, mapMutations } from 'vuex';
import getters from '../store/gettersMyAddon';
import mutations from '../store/mutationsMyAddon';

export default {
    name: 'MyAddon',
    components: {
        Tool
    },
    computed: {
        ...mapGetters('Tools/MyAddon', Object.keys(getters))
    },
    created() {
        this.$on('close', this.close);
    },
    /**
     * Put initialize here if mounting occurs after config parsing
     * @returns {void}
     */
    mounted() {
        this.initialize();
        if (this.isActive) {
            this.setActive(true);
        }
        this.activateByUrlParam();
        this.applyTranslationKey(this.name);
    },
    methods: {
        ...mapActions('Tools/MyAddon', ['activateByUrlParam', 'initialize']),
        ...mapMutations('Tools/MyAddon', Object.keys(mutations)),

        /**
         * Closes this tool window by setting active to false
         * @returns {void}
         */
        close() {
            this.setActive(false);

            // TODO replace trigger when Menu is migrated
            // set the backbone model to active false for changing css class in menu (menu/desktop/tool/view.toggleIsActiveClass)
            // else the menu-entry for this tool is always highlighted
            const model = Radio.request('ModelList', 'getModelByAttributes', {
                id: this.$store.state.Tools.VueAddon.id
            });

            if (model) {
                model.set('isActive', false);
            }
        }
    }
};
</script>

<template lang="html">
    <Tool
        :title="$t(name)"
        :icon="glyphicon-resize-full"
        :active="true"
        :render-to-window="false"
        :resizable-window="false"
        :deactivateGFI="false"
    >
        <template v-slot:toolBody>
            HALLO!!!!!!!!!!!
            <div
                v-if="active"
                id="vue-addon"
            >
                {{ $t('additional:modules.tools.vueAddon.content') }}
            </div>
        </template>
    </Tool>
</template>
