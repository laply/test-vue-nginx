<template>
  <div>
    <div class="board-title">Board List</div>
    <div class="board-body">
      <div v-if="loading">Page Loading ...</div>
      <div v-else>
        <div v-for="board in boards" :key="board.id">
          <router-link :to="'/b/' + board.id">
            <div class="board-item" :data-bgcolor="board.bgColor">
              <div class="board-item-title">{{ board.title }}</div>
            </div>
          </router-link>
        </div>
        <div class="board-item board-new-item" @click.prevent="TOGGLE_ADD_BOARD(true)">
          <div class="board-item-title">create new board</div>
        </div>
      </div>
    </div>
    <AddBoard v-if="isAddBoard" />
  </div>
</template>

<script>
import AddBoard from "./AddBoard.vue";
import { mapState, mapMutations, mapActions } from "vuex";

export default {
  name: "HomePage",
  components: {
    AddBoard,
  },
  data() {
    return {
      loading: false,
    };
  },
  created() {
    this.fetchData();
  },
  watch: {
    $route: {
      handler: "fetchData()",
      immediate: true,
    },
  },
  computed: {
    ...mapState(["isAddBoard", "boards"]),
  },
  updated() {
    Array.from(document.querySelectorAll(".board-item")).forEach((el) => {
      el.style.backgroundColor = el.dataset.bgcolor || "#ddd";
    });
  },
  methods: {
    ...mapMutations(["TOGGLE_ADD_BOARD"]),
    ...mapActions(["FETCH_BOARDS"]),
    fetchData() {
      this.loading = true;
      this.FETCH_BOARDS().finally(() => {
        this.loading = false;
      });
    },
  },
};
</script>

<style>
.board-title {
  margin: 20px;
  font-size: 30px;
}
.board-body {
  width: 100vw;
  display: flex;
  flex-direction: row;
  justify-content: start;
}
.board-item {
  width: 150px;
  height: 150px;
  margin: 20px;
  padding: 5px;
  border-radius: 10px;
  background-color: #fff;
  box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: all 0.3s ease;
}
.board-item-title {
  margin: 20px;
  font-size: 20px;
  text-align: center;
  color: #fff;
}
</style>
