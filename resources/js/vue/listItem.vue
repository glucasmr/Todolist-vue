<template>
    <div class="item">
        <input
            type="checkbox"
            @change="updateCheck()"
            :checked="item.completed"
            />
        <span :class="[item.completed ? 'completed' : '', 'itemText']"> {{ item.name }} </span>
        <button @click="removeItem()" class="trashCan">
            <font-awesome-icon icon="trash" />
        </button>
    </div>
</template>

<script>
export default {
    props: ['item'],
    methods: {
        updateCheck() {
            if( this.item.completed ) {
                this.item.completed = false;
            } else {
                this.item.completed = true;
            }
            axios.put('api/item/' + this.item.id, {
                item: this.item
            } )
            .then ( response => {
                if ( response.status == 200 ) {
                    this.$emit('itemchanged');
                }
            })
            .catch( error => {
                console.log( error );
            })
        },
        removeItem() {
            axios.delete('api/item/' + this.item.id)
            .then( response => {
                if( response.status == 200 ){
                    this.$emit('itemchanged');
                }
            })
            .catch( error => {
                console.log( error );
            })
        }
    }

}
</script>

<style scoped>
.completed {
    text-decoration: line-through;
    color: #999999;
}
.itemText {
    width: 100%;
    margin-left: 20px;
}
.item {
    display: flex;
    justify-content: center;
    align-items: center;
}
.trashCan {
    background: #e6e6e6;
    border: none;
    color: #FF0000;
    outline: none;
}
</style>