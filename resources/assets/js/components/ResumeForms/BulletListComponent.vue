<template>
    <div class="form-bullet-list">
        <div class="form-group row">
            <div class="col-sm-4">
                <label class="col-form-label font-weight-bold"
                       v-bind:for="getHashedElementId('add-option')">Write your own options (Optional)</label>

                <p class="text-muted card-text">Write each individual bullet point, then click the "Save and Add
                    Another" button.</p>
            </div>

            <div class="col-sm-8">
                <textarea class="form-control" placeholder="Enter option..."
                          v-bind:id="getHashedElementId('add-option')"
                          v-model="text"></textarea>

                <div class="text-right py-3">
                    <button type="button" class="btn btn-light shadow-sm" aria-pressed="false"
                            v-on:click="addItem(text); text = ''">Save and Add Another
                    </button>
                </div>
            </div>
        </div>

        <div class="form-group row">
            <div class="col-sm-4">
                <label class="col-form-label font-weight-bold"
                       v-bind:for="getHashedElementId('review-options')">Review your options</label>
            </div>

            <div class="col-sm-8">
                <ul class="list-group"
                    v-if="formData.items.length > 0">
                    <li class="list-group-item list-group-item-action d-flex justify-content-between align-items-center"
                        v-for="(item, index) in formData.items"
                        v-bind:index="index"
                        v-bind:key="index">
                        <textarea class="form-control" cols="30" rows="1" placeholder="Enter option..."
                            v-model="editItemText"
                            v-if="editItemIndex === index"></textarea>
                        <span v-text="item" v-else></span>

                        <div class="buttons ml-3" v-if="editItemIndex === index">
                            <button type="button" class="btn btn-sm btn-light"
                                v-on:click="saveItem()">
                                <span class="fa-check"></span>
                            </button>

                            <button type="button" class="btn btn-sm btn-link close ml-2 mt-1" aria-label="Close"
                                v-on:click="cancelEditing()">
                                <span aria-hidden="true">&times;</span>
                            </button>
                        </div>

                        <div class="buttons ml-3" v-else>
                            <button type="button" class="btn btn-sm btn-light"
                                v-on:click="editItem(item, index)">
                                <span class="fa-pencil"></span>
                            </button>

                            <button type="button" class="btn btn-sm btn-link ml-2 mt-1 text-dark"
                                v-on:click="removeItem(index)">
                                <span class="fa-trash"></span>
                            </button>
                        </div>
                    </li>
                </ul>

                <div class="alert alert-warning" role="alert"
                     v-else>
                    No option is added yet. Please type something and click on<br><strong>"Save and Add
                    Another"</strong> button to add one.
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import ComponentHashMixin from "./../../mixins/ComponentHashMixin.js";
    import HandleFormDataMixin from "./../../mixins/HandleFormDataMixin.js";

    export default {
        data() {
            return {
                editItemIndex: null,
                editItemText: "",

                formData: {
                    items: []
                },

                text: ""
            };
        },

        methods: {
            /**
             * Adds a new item to the list.
             * 
             * @param   {String} item
             * 
             * @returns {void}
             */
            addItem(item) {
                if (item.length === 0) {
                    return;
                }

                this.formData.items.push(item);
            },

            /**
             * Cancel the editing of the item.
             * 
             * @return {void}
             */
            cancelEditing() {
                this.editItemIndex = null;
                this.editItemText  = '';
            },

            /**
             * Edits the item stored at the index.
             * 
             * @param  {String} item
             * @param  {Number} index
             * 
             * @return {void}
             */
            editItem(item, index) {
                this.editItemIndex = index;
                this.editItemText  = item;
            },

            /**
             * Removes the item from the list by the supplied index.
             *
             * @param   {Number} index
             *
             * @returns {void}
             */
            removeItem(index) {
                this.formData.items.splice(index, 1);
            },

            /**
             * Saves the editing item into the items list.
             * 
             * @return {void}
             */
            saveItem() {
                Vue.set(this.formData.items, this.editItemIndex, this.editItemText);

                this.cancelEditing();
            }
        },

        mixins: [
            ComponentHashMixin,
            HandleFormDataMixin
        ]
    };
</script>

<style scoped>
    .buttons {
        min-width: 75px;
    }
</style>
