<template>
    <div class="feature_A">
        <!-- wwManager:start -->
        <wwSectionEditMenu v-bind:sectionCtrl="sectionCtrl"></wwSectionEditMenu>
        <!-- wwManager:end -->

        <wwObject class="background" v-bind:ww-object="section.data.background" ww-category="background"></wwObject>

        <div class="container section-padding">
            <div class="row">
                <div class="block-header">
                    <h1 class="section-title">
                        <wwObject v-bind:ww-object="section.data.title"></wwObject>
                    </h1>
                    <h2 class="section-subtitle">
                        <wwObject v-bind:ww-object="section.data.subtitle"></wwObject>
                    </h2>
                </div>
            </div>

            <div class="row">
                <div class="block-content">
                    <div class="feature-block" v-for="block in section.data.blocks" :key="block.uniqueId">

                        <div class="block-img">
                            <wwObject v-bind:ww-object="block.img"></wwObject>
                        </div>
                        
                        <div class="block-title">
                            <wwObject v-bind:ww-object="block.title"></wwObject>
                        </div>
                        
                        <div class="block-text">
                            <wwObject v-bind:ww-object="block.text"></wwObject>
                        </div>

                        <div class="button-container">
                            <div class="button-wrapper">
                                <wwObject v-bind:ww-object="block.button"></wwObject>
                            </div>
                        </div>
                        
                        <!-- wwManager:start -->
                        <div v-show="editMode" class="edit-button-top-left" @click="removeBlock(block)">
                            <i class="wwi wwi-delete" aria-hidden="true"></i>
                        </div>
                        <!-- wwManager:end -->

                    </div>
                   
                    <!-- wwManager:start -->
                    <div v-show="editMode" class="add-block-container">
                        <div class="add-block" @click="addBlock()">
                            <i class="wwi wwi-add" aria-hidden="true"></i>
                        </div>
                    </div>
                    <!-- wwManager:end -->
                </div>
            </div>
        </div>

    </div>
</template>

<script>
export default {
    name: "feature_A",
    props: {
        sectionCtrl: Object
    },
    data() {
        return {
        }
    },
    computed: {
        section() {
            return this.sectionCtrl.get();
        },
        editMode() {
            return this.sectionCtrl.getEditMode() == 'CONTENT'
        }
    },
    created() {
        //Initialize section data
        this.section.data = this.section.data || {};

        if (!this.section.data.background) {
            this.section.data.background = wwLib.wwObject.getDefault({ type: 'ww-color' });
        }
        if (!this.section.data.blocks) {
            this.section.data.blocks = [this.getNewBlock(), this.getNewBlock(), this.getNewBlock()]
        }
        if (!this.section.data.title) {
            this.section.data.title = wwLib.wwObject.getDefault({ type: 'ww-text' });
        }
        if (!this.section.data.subtitle) {
            this.section.data.subtitle = wwLib.wwObject.getDefault({ type: 'ww-text' });
        }
        this.sectionCtrl.update(this.section);
    },
    methods: {
        getNewBlock() {
            return {
                img: wwLib.wwObject.getDefault({ type: 'ww-image' }),
                title: wwLib.wwObject.getDefault({ type: 'ww-text' }),
                text: wwLib.wwObject.getDefault({ type: 'ww-text' }),
                button: wwLib.wwObject.getDefault({ type: 'ww-button' }),
                uniqueId: wwLib.wwUtils.getUniqueId()
            }
        },
        addBlock() {
            this.section.data.blocks.push(this.getNewBlock())
            this.sectionCtrl.update(this.section);
        },
        removeBlock(block) {
            this.section.data.blocks.splice(this.section.data.blocks.indexOf(block), 1);
            this.sectionCtrl.update(this.section);
        }
    }
};
</script>

<style scoped>
.feature_A .container {
    width: 100%;
    position: relative;
}

.feature_A .background {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
}

.feature_A .block {
    margin-bottom: 20px;
}

.feature_A .block-img-container {
    width: 100%;
    position: relative;
    padding: 20px;
}

.feature_A .block-img {
    width: 100%;
}

.feature_A .block-title {
    margin-top: 10px;
}

.feature_A .section-subtitle {
    margin-top: 10px;
    margin-bottom: 20px;
}

.feature_A .block-text {
    margin-top: 10px;
}

.feature_A .button-container {
    text-align: center;
    width: 100%;
    margin-top: 40px;
    margin-bottom: 30px;
}

.feature_A .ww-add-block {
    height: 200px;
}

.feature_A .duplicate-button {
    left: 50px;
}

.feature_A .feature-block {
    position: relative;
    display: inline-block;
    vertical-align: top;
    width: 100%;
    padding: 15px;
}

.feature_A .row {
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
    flex-wrap: wrap;
    margin-right: -15px;
    margin-left: -15px;
}

.feature_A .block-header {
    position: relative;
    width: 100%;
    min-height: 1px;
    padding-right: 15px;
    padding-left: 15px;
    -ms-flex: 0 0 100%;
    flex: 0 0 100%;
    max-width: 100%;
}

.feature_A .block-content {
    position: relative;
    width: 100%;
    min-height: 1px;
    padding-right: 15px;
    padding-left: 15px;
    -ms-flex: 0 0 100%;
    flex: 0 0 100%;
    max-width: 100%;
}

.feature_A .edit-button-top-left {
    position: absolute;
    left: -2px;
    top: -17px;
    width: 36px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 100%;
    text-align: center;
    font-size: 18px;
    line-height: 36px;
    cursor: pointer;
    pointer-events: all;
    z-index: 3;
    color: white;
    background-color: #E73055;
    background: linear-gradient(to right, #E73055 0%, rgb(175, 33, 61) 100%);
}

.feature_A .section-padding {
    padding: 30px 15px
}

.feature_A .add-block-container {
    display: flex;
    justify-content: center;
}

.feature_A .add-block {
  color: white;
  cursor: pointer;
  pointer-events: all;
  font-size: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: 100%;
  background-color: #1763A9;
  background: linear-gradient(to right, #1763A9 0%, #2E85C2 100%);
}

@media (min-width: 480px) {
    .feature_A .block-content {
        -ms-flex: 0 0 83.333333%;
        flex: 0 0 83.333333%;
        max-width: 83.333333%;
        margin-left: 8.333333%;
    }
}

@media (min-width: 768px) {
    .feature_A .section-padding {
        padding: 50px 30px
    }
    .feature_A .feature-block {
        width: 33.333333%;
        padding: 15px;
    }
}

@media (min-width: 992px) {
    .feature_A .section-padding {
        padding: 75px 50px
    }
}

@media (min-width: 1024px) {
    .feature_A .block-content {
        -ms-flex: 0 0 66.333333%;
        flex: 0 0 66.333333%;
        max-width: 66.333333%;
        margin-left: 16.333333%;
    }
}
</style>
