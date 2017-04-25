<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-md-6 col-md-offset-3">
          <div class="lead-form">
            <h1 class="text-center">Postcode Information</h1>
            <hr />
            <div class="row">
              <div class="col-md-12">
                <input type="text" class="form-control" placeholder="Enter Postcode" v-model="startingZip">
                <span class="city-span text-success">{{errorLog}}</span>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12">
                <h1 class="text-center">Results</h1>
                <div class="results">
                  <table class="table table-hover table-bordered">
                    <tbody>
                      <tr  class="danger">
                        <td>Admin county</td>
                        <td>{{ adminCounty }}</td>
                      </tr>
                      <tr  class="danger">
                        <td>Admin district</td>
                        <td>{{ adminDistrict }}</td>
                      </tr>
                      <tr class="danger">
                        <td>Admin Ward</td>
                        <td>{{ adminWard }}</td>
                      </tr>
                      <tr class="success">
                        <td>Clinical Commissioning Group</td>
                        <td>{{ ccg }}</td>
                      </tr>
                      <tr class="success">
                        <td>PCT</td>
                        <td>{{ pct }}</td>
                      </tr>
                      <tr class="success">
                        <td>Strategic Health Authority</td>
                        <td>{{ nhsHa }}</td>
                      </tr>
                      <tr class="info">
                        <td>Euro electoral region</td>
                        <td>{{ euroER }}</td>
                      </tr>
                      <tr class="info">
                        <td>NUTS</td>
                        <td>{{ nuts }}</td>
                      </tr>
                      <tr>
                        <td>Parish</td>
                        <td>{{ parish }}</td>
                      </tr>
                      <tr>
                        <td>Parliamentary Constituancy</td>
                        <td>{{ parliamentConst }}</td>
                      </tr>
                      <tr>
                        <td>Region</td>
                        <td>{{ region }}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
          <!-- end of .lead-form -->
        </div>
        <!-- end of .col-md-6.col-md-offset-3 -->
      </div>
      <!-- end of .row -->
    </div>
    <!-- end of .container -->
  </div> <!-- end of end -->
</template>

<script>

export default {
  name: 'app',

    result: {
          startingZip: '',
          errorLog: ''
        },
      data() {
        return {
          startingZip: '',
          errorLog: '',
          adminCounty: '',
          adminDistrict: '',
          adminWard: '',
          ccg: '',
          euroER: '',
          nuts: '',
          parish: '',
          parliamentConst: '',
          pct: '',
          nhsHa: '',
          region: ''
        }
      },
      watch: {
        startingZip: function () {
          if (this.startingZip.length == 7) {
            this.lookupStartingZip()
          }
        }
      },
      methods: {
        lookupStartingZip: _.debounce(function () {
          var app = this
          app.errorLog = "Searching..."
          axios.get('https://api.postcodes.io/postcodes/' + app.startingZip)
            .then(function (response) {
              app.errorLog = "Success"
              app.adminCounty = response.data.result.admin_county
              app.adminDistrict = response.data.result.admin_district
              app.adminWard = response.data.result.admin_ward
              app.ccg = response.data.result.ccg
              app.euroER = response.data.result.european_electoral_region
              app.nuts = response.data.result.nuts
              app.parish = response.data.result.parish
              app.parliamentConst = response.data.result.parliamentary_constituency
              app.pct = response.data.result.primary_care_trust
              app.nhsHa = response.data.result.nhs_ha
              app.region = response.data.result.region
            })
            .catch(function (error) {
              console.log(error)
              app.errorLog = "Invalid Postcode"
            })
        }, 500)
      }
}
</script>
<style>
#app {
  height: 100%;
  width: 100vw;
  background-color: red;
  background: url("https://source.unsplash.com/category/nature/1920x1080") no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}

#submit-form {
  margin-top: 10px;
}

.lead-form {
  background-color: rgba(255, 255, 255, 0.6);
  border-radius: 5px;
  padding: 10px 50px 30px 50px;
  margin-top: 20px;
  margin-bottom: 20px;
}

span.city-span {
  color: (#444);
  margin-left: 5px;
  margin-top: 10px;
}

.form-control {
  margin-bottom: 3px;
}
</style>
