<template>
  <div>
    <Header />
    <hr />
    <div class="row justify-content-center">
      <h4>Danh sách đơn hàng của shop</h4>
    </div>
    <br />
    <b-card>
      <div class>
        <div class="row">
          <div class="col-md-6">
            <div class="row">
              <!-- <button class="btn btn-outline-success" type="button" style="margin-left:20px">tình trạng</button> -->
              <div class="col-md-8">
                <multiselect
                  v-model="value"
                  :options="options"
                  placeholder="Select one"
                  label="name"
                  track-by="name"
                ></multiselect>
              </div>
            </div>
          </div>
          <div class="col-md-6">
            <b-input-group>
              <template v-slot:prepend>
                <div>
                  <b-dropdown :text="sortBy.col" variant="outline-success">
                    <b-dropdown-item
                      :value="sortModel[1].col"
                      @click="sortBy=sortModel[0]"
                    >ngày giao</b-dropdown-item>
                    <b-dropdown-item
                      :value="sortModel[0].col"
                      @click="sortBy=sortModel[1]"
                    >ngày nhập</b-dropdown-item>
                  </b-dropdown>
                  <b-dropdown :text="sortDesc==true?'giảm':'tăng'" variant="outline-success">
                    <b-dropdown-item @click="sortDesc=false">tăng</b-dropdown-item>
                    <b-dropdown-item @click="sortDesc=true">giảm</b-dropdown-item>
                  </b-dropdown>
                </div>
              </template>

              <b-form-input
                v-model="filter"
                type="search"
                id="filterInput"
                placeholder="Type to Search"
              ></b-form-input>
              <template v-slot:append>
                <b-button variant="outline-success" type="button">Search</b-button>
              </template>
            </b-input-group>
          </div>
        </div>
        <br />
        <b-table
          id="my-table"
          :items="items"
          :fields="fields"
          :per-page="perPage"
          :current-page="currentPage"
          small
          :sort-by.sync="sortBy.key"
          :sort-desc.sync="sortDesc"
          sort-icon-left
          :filter="filter"
        >
          <template v-slot:cell(show_details)="row">
            <b-button :to="'/'" size="sm" class="mr-2">Details</b-button>
          </template>
        </b-table>
        <div class="mt-3">
          <p class="mt-3">Current Page: {{ currentPage }}</p>
          <b-pagination
            v-model="currentPage"
            :total-rows="rows"
            :per-page="perPage"
            align="center"
            first-text="First"
            prev-text="Prev"
            next-text="Next"
            last-text="Last"
          ></b-pagination>
        </div>
      </div>
    </b-card>
  </div>
</template>
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<script>
import Header from "~/components/Header.vue";
import Multiselect from "vue-multiselect";

export default {
  components: {
    Header: Header,
    Multiselect: Multiselect
  },
  data() {
    return {
      value: { name: "Tình trạng đơn hàng", language: "JavaScript" },
      options: [
        { name: "đang xử lý", language: "JavaScript", $isDisabled: true },
        { name: "Rails", language: "Ruby" },
        { name: "Sinatra", language: "Ruby" },
        { name: "Laravel", language: "PHP" },
        { name: "Phoenix", language: "Elixir" }
      ],
      filter: null,
      // filterOn: "",
      sortModel: [
        { key: "first_name", col: "ngày giao" },
        { key: "last_name", col: "ngày nhập" }
      ],
      sortBy: { key: "first_name", col: "ngày cắm" },

      sortDesc: false,
      orderBy: "",
      fields: [
        { key: "id", label: "ID", sortable: false },
        { key: "first_name", label: "Ngày giao", sortable: true },
        { key: "last_name", label: "Ngày nhập", sortable: true },
        { key: "status", label: "Trạng thái", sortable: false },
        { key: "show_details", label: "Details", sortable: false }
      ],
      Filter: [
        {
          active: true,
          id: 0,
          name: "ngày giao"
        },
        {
          active: false,
          id: 1,
          name: "ngày nhập"
        }
      ],
      type: [
        {
          active: true,
          id: 2,
          name: "trạng thái",
          orderBy: [
            { active: true, title: "asc", description: "đang xử lý" },
            { active: false, title: "desc", description: "chờ giao" },
            { active: false, title: "delivery", description: "đang giao" },
            { active: false, title: "success", description: "đã giao" },
            { active: false, title: "cancel", description: "hủy" }
          ]
        }
      ],
      perPage: 10,
      currentPage: 1,
      items: [
        { id: 8, first_name: "2019/10/3", last_name: "Slate" },
        { id: 9, first_name: "2020/11/3", last_name: "Slaghoople" },
        { id: 9, first_name: "2020/11/11", last_name: "Slaghoople" },
        { id: 9, first_name: "2020/3/23", last_name: "Slaghoople" }
      ]
      //  items: [
      //   { id: 8, first_name: "1", last_name: "Slate" },
      //   { id: 9, first_name: "2", last_name: "Slaghoople" },
      //   { id: 22, first_name: "Bamm Bamm", last_name: "Rubble" },
      //   { id: 17, first_name: "The Great", last_name: "Gazzoo" },
      //   { id: 83, first_name: "Rockhead", last_name: "Slate" },
      //   { id: 39, first_name: "Pearl", last_name: "Slaghoople" },
      //   { id: 92, first_name: "Pearl", last_name: "Slaghoople" },
      //   { id: 222, first_name: "Bamm Bamm", last_name: "Rubble" },
      //   { id: 217, first_name: "The Great", last_name: "Gazzoo" },
      //   { id: 283, first_name: "Rockhead", last_name: "Slate" },
      //   { id: 329, first_name: "Pearl", last_name: "Slaghoople" }
      // ]
    };
  },
  computed: {
    rows() {
      return this.items.length;
    },
    filteredEvents() {
      return this.Filter.filter(event => {
        return event.active == true;
      });
    }
  },
  methods: {
    isActiveDropdown(id) {
      this.Filter.forEach((item2, index) => {
        if (index == id) {
          this.Filter[index].active = true;
        }
        if (index != id) {
          this.Filter[index].active = false;
        }
      });
    }
  }
};
</script>

