<template>
  <div>
    <div class="row justify-content-center">
      <h3>Chi tiết đơn hàng</h3>
    </div>
    <b-card bg-variant="light">
      <b-form-group>
        <div class="row">
          <label class="col-md-6 text-right">khách hàng:</label>
          {{ordersDetail.Customer}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Số điện thoại:</label>
          {{ordersDetail.PhoneNumber}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Chủ đề:</label>
          {{ordersDetail.Theme}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Link sản phẩm:</label>
          {{ordersDetail.LinkProduct}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Tên người nhận:</label>
          {{ordersDetail.Recipient}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Địa chỉ:</label>
          {{ordersDetail.Address}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Ghi chú:</label>
          {{ordersDetail.Note}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Số lượng:</label>
          {{ordersDetail.Quantity}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Giá:</label>
          {{ordersDetail.Price}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Phí ship:</label>
          {{ordersDetail.ShippingFee}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Ngày đặt:</label>
          {{ordersDetail.OrderDate}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Ngày nhận:</label>
          {{ordersDetail.DeliveryDatetime}}
        </div>
        <div class="row">
          <label class="col-md-6 text-right">Trạng thái đơn hàng:</label>
          {{ordersDetail.orderstatus.name}}
        </div>

        <b-form-group
          v-if="ordersDetail.orderstatus.type==3"
          label-cols-sm="6"
          class="upload"
          label="File ảnh thật"
          label-align-sm="right"
          label-for
        >
          <div>
            <b-form-file
              v-model="file"
              :state="Boolean(file)"
              placeholder="Choose a file or drop it here..."
              drop-placeholder="Drop file here..."
            ></b-form-file>
            <div style="padding-top:5px">
              <b-button variant="outline-success" @click="upload1" class="mr-2">Upload</b-button>
              <a v-if="ordersDetail.ImageUrl1!='null'" :href="ordersDetail.ImageUrl1">click để xem</a>
            </div>
          </div>
        </b-form-group>
        <b-form-group
          v-if="ordersDetail.orderstatus.type==6"
          label-cols-sm="6"
          class="upload"
          label="File ảnh chứng thực"
          label-align-sm="right"
          label-for
        >
          <div>
            <b-form-file
              v-model="file2"
              :state="Boolean(file)"
              placeholder="Choose a file or drop it here..."
              drop-placeholder="Drop file here..."
            ></b-form-file>
            <div style="padding-top:5px">
              <b-button variant="outline-success" @click="upload2" class="mr-2">Upload</b-button>
              <a v-if="ordersDetail.ImageUrl2!='null'" :href="ordersDetail.ImageUrl2">click để xem</a>
            </div>
          </div>
        </b-form-group>

        <div class="row justify-content-center">
          <b-button
            v-if="ordersDetail.orderstatus.type==1"
            variant="outline-success"
            @click="clickApprove"
          >Nhận</b-button>
          <b-button v-if="ordersDetail.orderstatus.type==1" @click="clickReject">Không nhận</b-button>
          <b-button
            v-if="ordersDetail.orderstatus.type==3"
            variant="outline-success"
            @click="clickCompleted"
          >Hoàn thiện</b-button>
          <b-button
            v-if="ordersDetail.orderstatus.type==5"
            variant="outline-success"
            @click="clickShipping"
          >Giao hàng</b-button>
          <b-button
            v-if="ordersDetail.orderstatus.type==6"
            variant="outline-success"
            @click="clickCustomerReceive"
          >Khách đã nhận</b-button>
          <b-button
            v-if="ordersDetail.orderstatus.type==8"
            variant="success"
            disabled
          >Khách hàng đã nhận</b-button>
        </div>
        <div>
          <b-alert
            :show="dismissCountDown"
            dismissible
            variant="warning"
            @dismissed="dismissCountDown=0"
            @dismiss-count-down="countDownChanged"
          >
            <p>{{Notify}}</p>
            <b-progress variant="warning" :max="dismissSecs" :value="dismissCountDown" height="3px"></b-progress>
          </b-alert>
        </div>
        <hr />
      </b-form-group>
    </b-card>
  </div>
</template>
<style scoped>
.upload {
  padding-right: 10px !important;
}
</style>
<script>
import axios from "~/plugins/axios";
export default {
  components: {},
  data() {
    return {
      Notify: "",
      dismissSecs: 10,
      dismissCountDown: 0,
      showDismissibleAlert: false,
      file: null,
      file2: null,
      ordersDetail: {
        _id: "",
        Customer: "",
        PhoneNumber: "",
        createdAt: "",
        updatedAt: "",
        __v: 0,
        orderstatus: {
          _id: "",
          name: "",
          description: "",
          createdAt: "",
          updatedAt: "",
          __v: 0,
          priority: "",
          id: ""
        },
        shop: {
          _id: "5db1d1ad5eadc03350b682cf",
          ShopName: "Shop 1",
          createdAt: "2019-10-24T16:30:37.921Z",
          updatedAt: "2019-11-02T05:00:54.489Z",
          __v: 0,
          Address: "địa chỉ 1",
          PhoneNumber: "674643643534",
          ShopOwner: "",
          order: "5dbc0805915b5d3cc071ac59",
          id: "5db1d1ad5eadc03350b682cf"
        },
        id: "5dbfa4f8972f68440c6886f2"
      },
      options: []
    };
  },
  created() {
    this.getOrderDetail();
    this.getOptions();
  },
  computed: {
    id() {
      return this.$route.params.id;
    }
  },
  methods: {
    countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown;
    },
    showAlert(Notify) {
      this.Notify = Notify;
      this.dismissCountDown = this.dismissSecs;
    },
    update(ordersDetail) {
      axios
        .put(`/orders/${this.id}`, ordersDetail)
        .then(response => {
          console.log(response);
          this.ordersDetail = response.data;
        })
        .catch(e => {
          this.errors.push(e);
        });
    },
    upload1() {
      const formData = new FormData();
      formData.append("files", this.file);
      axios
        .post(`/upload`, formData, {
          headers: { "Content-Type": "multipart/form-data" }
        })
        .then(res => {
          this.showAlert("Thành công!");
          this.ordersDetail.ImageUrl1 = res.data[0].url;
          this.update(this.ordersDetail);
        })
        .catch(err => {
          alert("Lỗi!!!");
          console.log(err);
        });
    },
    upload2() {
      const formData2 = new FormData();
      formData2.append("files", this.file2);
      axios
        .post(`/upload`, formData2, {
          headers: { "Content-Type": "multipart/form-data" }
        })
        .then(res => {
          this.showAlert("Thành công!");
          this.ordersDetail.ImageUrl2 = res.data[0].url;
          this.update(this.ordersDetail);
        })
        .catch(err => {
          alert("Lỗi!!!");
          console.log(err);
        });
    },
    getOrderDetail() {
      let getOrderDetail = axios
        .get(`/orders/${this.id}`)
        .then(response => {
          console.log(response);
          this.ordersDetail = response.data;
        })
        .catch(e => {
          this.errors.push(e);
        });
    },
    clickCustomerReceive() {
      this.ordersDetail.orderstatus = this.options[6];
      let clickReject = axios
        .put(`/orders/${this.id}`, this.ordersDetail)
        .then(response => {
          console.log(response);
          this.ordersDetail = response.data;
        })
        .catch(e => {
          this.errors.push(e);
        });
    },
    clickShipping() {
      this.ordersDetail.orderstatus = this.options[5];
      let clickReject = axios
        .put(`/orders/${this.id}`, this.ordersDetail)
        .then(response => {
          console.log(response);
          this.ordersDetail = response.data;
        })
        .catch(e => {
          this.errors.push(e);
        });
    },
    clickCompleted() {
      if (this.ordersDetail.ImageUrl1 == "null") {
        this.showAlert("Ảnh chưa được upload");
      } else {
        this.ordersDetail.orderstatus = this.options[3];
        let clickReject = axios
          .put(`/orders/${this.id}`, this.ordersDetail)
          .then(response => {
            console.log(response);
            this.ordersDetail = response.data;
          })
          .catch(e => {
            this.errors.push(e);
          });
      }
    },
    clickApprove() {
      this.ordersDetail.orderstatus = this.options[2];
      let clickApprove = axios
        .put(`/orders/${this.id}`, this.ordersDetail)
        .then(response => {
          console.log(response);
          this.ordersDetail = response.data;
        })
        .catch(e => {
          this.errors.push(e);
        });
    },

    clickReject() {
      this.ordersDetail.orderstatus = this.options[1];
      let clickReject = axios
        .put(`/orders/${this.id}`, this.ordersDetail)
        .then(response => {
          console.log(response);
          this.ordersDetail = response.data;
        })
        .catch(e => {
          this.errors.push(e);
        });
    },
    orderBy(object,key) {
      object.sort((a, b) => Number(a.type) - Number(b.type))
    },
    getOptions() {
      let getOptions = axios
        .get(`/orderstatuses`)
        .then(response => {
          this.orderBy(response.data, "type");
          console.log(response.data);
          this.options = response.data;
        })
        .catch(e => {
          this.errors.push(e);
        });
    },
    handleFileUpload() {}
  }
};
</script>