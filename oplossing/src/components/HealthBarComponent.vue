<template lang="html">
    <section class="container">
        <h2>{{ this.name }} Health</h2>
        <div class="healthbar">
            <div class="healthbar__value" v-bind:style="barStyle"></div>
        </div>
    </section>
</template>
<script>
export default {
    props: [
        'name',
        'hp'
    ],
    computed: {
        barStyle() {
            if(this.hp < 0)
                return 'width: 0%';
            
            return `width: ${this.hp}%`
            //return "width: " + this.hp + "%"
        }
    }
}
</script>
<style scoped lang="scss">
    @use '../../scss/global' as *;

    .healthbar {
        width: 100%;
        height: 40px;
        border: 1px solid $color-secondary;
        margin: 1rem 0;
        background: $color-tertiary;

        &__value {
            background-color: $color-health;
            width: 100%;
            height: 100%;
        }
    }

    h2 {
        margin: 0.25rem;
    }
</style>