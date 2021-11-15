<template>
  <div class="container">
    <br /><br />
    <div class="row">
      <div>
        <h2>Add New Customer</h2>
        <br />
      </div>
    </div>
   <div class="container">

        <div class="form-group row">
            
            <div class="col">
                <div class="form-group">
                    <label for="customerName">CustomerName :</label>
                    <input type="text" v-model="customers.customerName" class="form-control" id="customerName" placeholder="Enter customerName" name="customerName">
                </div>
                <div class="form-group">
                    <label for="customerAddress">CustomerAddress :</label>
                    <input type="text" v-model="customers.customerAddress" class="form-control" id="customerAddress" placeholder="Enter customerAddress" name="customerAddress">
                </div>
                <div class="form-group">
                    <label for="customerAge">CustomerAge :</label>
                    <input type="text" v-model="customers.customerAge" class="form-control" id="customerAge" placeholder="Enter customerAge" name="customerAge">
                </div>
            </div>
        </div>
        
      <div class="form-group row">
        <div class="col">
          <div class="form-group">
            <label for="productPicture">Upload Image:</label>
            <UploadImage
              id="customerPicture"
              name="customerPicture"
              address="customerPicture"
            />
          </div>
        </div>
      </div>
      <div class="form-group row">
            <div class="col">
                <div class="form-group">
                    <label for="customerPicture">CustomerPicture:</label>
                    <textarea v-model="customer.CustomerPicture" class="form-control" id="customerPicture" placeholder="Enter customerPicture" rows="3"></textarea>
                </div>
            </div>
        </div>

      <button class="btn btn-primary" v-on:click="SaveBook()">Save</button
      >&nbsp;
      <button class="btn btn-danger" v-on:click="Cancel()">Cancel</button>
    </div>
    <br /><br />
  </div>
</template>

<script>
import UploadImage from "./UploadImage.vue";
import axios from "axios";
//import moment from "moment";
export default {
  name: "AddCustomer",
  components: {
    UploadImage,
  },
  data() {
    return {
      accessToken:"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE2MzY3ODg1ODMsImV4cCI6MTYzNjc5MjE4M30.VlaL9lL9STCuJoHCutDfccuGXevXCO1JAqjnYUAf7KI",
      customer: {
        customerName: "",
        customerAddress: "",
        customerAge: 1,
        customerPicture : "",
      },
    };
  },
  methods: {
    async SaveBook() {
      if (confirm("Do you want to save this area?")) {

        let customerPicture = await this.$refs.customerPicture.getFileName();

        if ((await customerPicture) !== "") {
          this.customer.customerPicture = await customerPicture;
          await this.$refs.customerPicture.UploadImage();
        }

        await axios.post(this.$apiUrl + "customer", this.customer, {
          headers: { Authorization: `bearer ${this.accessToken}` },
        });
        await this.$router.push("/customer");
      }
    },
    Cancel() {
      if (confirm("Do you want to cancel adding this customer?")) {
        this.$router.push("/");
      }
    },
  },
};
</script>

<style scoped>
.book-item {
  background: #f4f4f4;
  padding: 10px;
  border-bottom: 1px #ccc dotted;
}

label {
  /* Other styling... */
  text-align: right;
  clear: both;
  float: left;
  margin-right: 15px;
}
</style>
