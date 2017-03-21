<template>
    <div>
        <section>
            <h2>CSS transitions</h2>
            <button v-on:click="show = !show">Toggle</button>
            <transition name="fade" appear>
                <div class="elem" v-if="show">Hello!</div>
            </transition>
        </section>

        <section>
            <h2>CSS animations</h2>
            <button v-on:click="show = !show">Toggle</button>
            <transition name="bounce">
                <div class="elem" v-if="show">Hello!</div>
            </transition>
        </section>

        <h2>transition modes</h2>

        <section>
            <h3>default</h3>
            <button v-on:click="show = !show">
                <transition name="button">
                    <span v-bind:key="show">
                        {{ show ? 'Hide' : 'Show' }}
                    </span>
                </transition>
            </button>
        </section>
        <section>
            <h3>out-in</h3>
            <button v-on:click="show = !show">
                <transition name="button" mode="out-in">
                    <span v-bind:key="show">
                        {{ show ? 'Hide' : 'Show' }}
                    </span>
                </transition>
            </button>
        </section>
        <section>
            <h3>in-out</h3>
            <button v-on:click="show = !show">
                <transition name="button" mode="in-out">
                    <span v-bind:key="show">
                        {{ show ? 'Hide' : 'Show' }}
                    </span>
                </transition>
            </button>
        </section>

        <h2>transition-groups</h2>
        <p>Click an element to remove</p>

        <section>
            <button v-on:click="add">Press me often!!!</button>
            <button v-on:click="shuffle">Shuffle</button>
            <transition-group name="list" tag="ul">
                <li class="item" v-for="item, index in items" v-bind:key="item" v-on:click="remove(index)">{{ item }}</li>
            </transition-group>
        </section>

        <h3>As grid</h3>
        <section>
            <transition-group name="auto-list" tag="ul" class="blocks">
                <li class="item" v-for="item, index in items" v-bind:key="item" v-on:click="remove(index)">{{ item }}</li>
            </transition-group>
        </section>
    </div>
</template>

<script>
  export default {
    data: function() {
      return {
        count: 3,
        show: true,
        show2: false,
        items: ['1', '2', '3']
      }
    },
    methods: {
      add: function() {
        let array = this.items;

        if (array.length >= 24) {
          let index = Math.floor(Math.random() * array.length);
          array.splice(index, 1);
        }

        let index = Math.floor(Math.random() * array.length);
        array.splice(index, 0, ++this.count);

        if (this.count >= 999) {
          this.count = 1;
        }
      },
      remove: function(index) {
        this.items.splice(index, 1);
      },
      shuffle: function() {
        let a = this.items.slice();
        for (let i = a.length; i; i--) {
          let j = Math.floor(Math.random() * i);
          [a[i - 1], a[j]] = [a[j], a[i - 1]];
        }
        this.items = a;
      }
    }
  }
</script>

<style lang="sass" scoped>
    $time: 0.3s;

    span {
        display: inline-block;
    }

    .elem {
        transform: translate(0, 0);
        display: inline-block;
    }

    button {
        overflow: hidden;
        position: relative;
    }

    ul {
        list-style: none;
        margin: 15px 0 0 0;
        padding: 0;
        font-size: 18px;

        li {
            display: inline-block;
            padding: 2px 5px;
        }
    }

    section {
        padding: 10px;
    }

    .blocks {
        display: block;
        max-width: 300px;
        > li {
            box-sizing: border-box;
            display: inline-block;
            width: 33.33%;
            border: 1px solid #ccc;
            padding: 10px;
            text-align: center;
        }
    }

    .fade-enter-active, .fade-leave-active,
    .button-enter-active, .button-leave-active {
        transition: all $time;
    }

    .fade-enter {
        transform: translate(-100%, 0);
        opacity: 0;
    }

    .fade-leave-to {
        transform: translate(100%, 0);
        opacity: 0;
    }

    .bounce-enter-active {
        animation: bounce-in .5s;
    }
    .bounce-leave-active {
        animation: bounce-out .25s;
    }

    .button-enter {
        transform: translate(0, 100%);
        opacity: 0;
    }

    .button-leave-to {
        transform: translate(0, -100%);
        opacity: 0;
    }

    @keyframes bounce-in {
        0% {
            transform: scale(0) rotate(360deg);
        }

        50% {
            transform: scale(1.2) rotate(-45deg);
        }

        100% {
            transform: scale(1) rotate(0);
        }
    }

    @keyframes bounce-out {
        0% {
            transform: scale(1);
        }

        50% {

        }

        100% {
            transform: scale(0);
        }
    }

    .item {
        transition: all 0.3s;
        display: inline-block;

        &:hover {
            color: darkred;
            cursor: pointer;
        }
    }

    .list-enter, .list-leave-to {
        opacity: 0;
        transform: translate(0, -100%);
    }
    .list-leave-to {
        transform: translate(0, 100%);
    }
    .list-leave-active {
        position: absolute;
    }

    .auto-list-enter, .auto-list-leave-to {
        opacity: 0;
        transform: scale(0);
    }
    .auto-list-leave-to {
        opacity: 0;
        transform: scale(1.5);
        background: red;
    }
    .auto-list-leave-active {
        position: absolute;
    }
</style>