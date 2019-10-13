<template>
    <li class="memo-item">
        <strong>{{ memo.title }}</strong>
        <p @dblclick="handleDblClick">
            <template v-if="!isEditing">{{ memo.content }}</template>
            <input v-else type="text" ref="content" :value="memo.content" @blur="handleBlur" @keydown.enter="updateMemo" />
        </p>
        <button type="button" @click="deleteMemo"><i class="fas fa-times"></i></button>
    </li>
</template>

<script>
export default {
    name: "Memo",
    data () {
        return {
            isEditing: false
        }
    },
    beforeUpdate () {
        console.log("beforeUpdate =>", this.$refs.content);
    }, 
    updateed () {
        console.log("updated =>", this.$refs.content);
    },
    
    props: {
        memo: {
            type: Object
        }
    },
    methods: {
        deleteMemo() {
            const id = this.memo.id;
            this.$emit('deleteMemo', id);
        }, 
        handleDblClick() {
            this.isEditing = true;
            this.$nextTick(() => {
                this.$refs.content.focus();
            });
            
        },
        updateMemo (e) {
            const id = this.memo.id;
            const content = e.target.value.trim();
            if(content.length <= 0) {
                return false;
            }
            this.$emit('updateMemo', { id, content });
            this.isEditing = false;
        }, 
        handleBlur () {
            this.isEditing = false;
        }
    }, 
}
</script>

<style scoped>
.memo-form {
    margin-bottom: 24px;
    padding-bottom: 40px;
    border-bottom: 1px solid #eee;
}
.memo-form form fieldset div {
    position: relative;
    padding: 24px;
    margin-bottom: 20px;
    box-shadow: 0 4px 10px -4px rgba(0, 0, 0, 0.2);
    background-color: #fff;
}
.memo-form form fieldset div button[type="reset"] {
    position: absolute;
    right: 20px;
    bottom: 20px;
    font-size: 16px;
    background: none;
}
.memo-form form fieldset button[type="submit"] {
    float: right;
    width: 96px;
    padding: 12px 0;
    border-radius: 4px;
    background-color: #ff5a00;
    color: #fff;
    font-size: 16px; 
}
.memo-form form fieldset .memo-form__title-form {
    width: 100%;
    margin-bottom: 12px;
    font-size: 18px;
    line-height: 26px;
}
.memo-form form fieldset .memo-form__content-form {
    width: 100%;
    height: 66px;
    font-size: 14px;
    line-height: 22px;
    vertical-align: top;
}
.memo-form input:focus {
    outline: none;
}

.memo-item {
    overflow: hidden;
    position: relative;
    margin-bottom: 20px;
    padding: 24px;
    box-shadow: 0 4px 10px -4px rgba(0, 0, 0, 0.2);
    background-color: #fff;
    list-style: none;
}
.memo-item button {
    background: none;
    position: absolute;
    right: 20px;
    top: 20px;
    font-size: 20px;
    color: #e5e5e5;
    border: 0;
}
.memo-item strong {
    display: block;
    margin-bottom: 12px;
    font-size: 18px;
    font-weight: normal;
    word-break: break-all;
}
.memo-item p {
    font-size: 14px;
    line-height: 22px;
    color: #666;
}

.memo-item p input[type="text"] {
    box-sizing: border-box;
    width: 100%;
    font-size: inherit;
    border: 1px solid #999;
}
</style>