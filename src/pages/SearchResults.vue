<template>
  <div class="treroad-searchResults-page">
    <div class="treroad-searchResults-resultHeader">
      <div class="treroad-searchResults-queryCondition">
        <p class="treroad-searchResults-trainRoute">{{selectStation.departureStation}} <img src="../assets/arrow.png" alt=""> {{selectStation.arrivalStation}}</p>
        <p class="treroad-searchResults-searchTime">{{this.searchTime.day[0]}}年{{this.searchTime.day[1]}}月{{this.searchTime.day[2]}}日({{searchTime.week}}) {{searchTime.time.hour}}:{{searchTime.time.minute}}</p>
      </div>
      <router-link to="/home"><button>重新查詢</button></router-link>
    </div>
    <div class="treroad-searchResults-searchResultBackground">
        <div class="treroad-searchResults-searchResult">
          <div class="treroad-searchResults-resultFilter">
            <input type="radio" name="filter" id="all" checked>
            <label for="all">全部</label>
            <input type="radio" name="filter" id="timeOfArrival">
            <label for="timeOfArrival">到達時間</label>
            <input type="radio" name="filter" id="atLeastInterchange">
            <label for="atLeastInterchange">最少轉乘</label>
            <input type="radio" name="filter" id="rideTime">
            <label for="rideTime">行車時間</label>
          </div>
          <div v-for="(shift, index) in shiftList" class="treroad-searchResults-searchResultList" @click="toggleShowInformation(shift)">
            <div class="treroad-searchResults-trainInformation" id="trainInformation">
              <img class="treroad-searchResults-trainInformation-trainIcon" src="../assets/train-icon.png" alt="train">
              <div class="treroad-searchResults-trainInformation-mobileTrainLabel"></div>
              <div class="treroad-searchResults-trainInformation-trips">
                <p class="treroad-searchResults-trainInformation-title">區間{{shift.trainInformation.trainNumber}}</p>
                <p class="treroad-searchResults-trainInformation-value">{{shift.trainInformation.departureTime}} - {{shift.trainInformation.arrivalTime}}</p>
              </div>
              <div class="treroad-searchResults-trainInformation-driveTime">
                <p class="treroad-searchResults-trainInformation-title">行車時間</p>
                <p class="treroad-searchResults-trainInformation-value">{{shift.trainInformation.travelTime}}</p>
              </div>
              <div class="treroad-searchResults-trainInformation-fare">
                <p class="treroad-searchResults-trainInformation-title">費用</p>
                <p class="treroad-searchResults-trainInformation-value">{{shift.trainInformation.price}}</p>
              </div>
              <div class="treroad-searchResults-trainInformation-numberOfTransfer">
                <p class="treroad-searchResults-trainInformation-title">轉乘</p>
                <p class="treroad-searchResults-trainInformation-value">{{shift.trainInformation.transfer}}</p>
              </div>
              <img class="treroad-searchResults-trainInformation-downIcon" src="../assets/down.png" alt="down">
            </div>
            <div class="treroad-searchResults-transferInformation">
              <div v-for="(transferInformation, index) in shift.transferInformation" class="treroad-searchResults-transferInformation-transferArea">
                <div v-if="index == 0" class="treroad-searchResults-transferInformation-departureStation">
                  <p class="treroad-searchResults-transferInformation-departureTime">{{transferInformation.departureTime}}</p>
                  <span class="treroad-searchResults-transferInformation-circle"></span>
                  <p class="treroad-searchResults-transferInformation-station">{{transferInformation.departureStation}}</p>
                  <span class="treroad-searchResults-transferInformation-departureLine"></span>
                </div>
                <div class="treroad-searchResults-transferInformation-singleTrainInformation">
                  <img src="../assets/train-icon.png" alt="train">
                  <p ><span>區間{{transferInformation.trainNumber}}</span><span>{{transferInformation.travelTime}}</span><span>{{transferInformation.price}}元</span></p>
                  <span class="treroad-searchResults-transferInformation-arrivalthoughLine"></span>
                </div>
                <div v-if="index != shift.transferInformation.length - 1" class="treroad-searchResults-transferInformation-transferStation">
                  <p class="treroad-searchResults-transferInformation-arrivalTime">{{transferInformation.arrivalTime}}</p>
                  <p class="treroad-searchResults-transferInformation-departureTime">{{shift.transferInformation[index + 1].departureTime}}</p>
                  <p class="treroad-searchResults-transferInformation-transfer">轉乘</p>
                  <p class="treroad-searchResults-transferInformation-station">{{transferInformation.arrivalStation}}</p>
                  <span class="treroad-searchResults-transferInformation-circle"></span>
                  <span class="treroad-searchResults-transferInformation-circle"></span>
                  <span class="treroad-searchResults-transferInformation-departureLine"></span>
                  <span class="treroad-searchResults-transferInformation-arrivalLine"></span>
                </div>
                <div v-if="index == shift.transferInformation.length - 1" class="treroad-searchResults-transferInformation-arrivalStation">
                  <p class="treroad-searchResults-transferInformation-arrivalTime">{{transferInformation.arrivalTime}}</p>
                  <span class="treroad-searchResults-transferInformation-circle"></span>
                  <p class="treroad-searchResults-transferInformation-station">{{transferInformation.arrivalStation}}</p>
                  <span class="treroad-searchResults-transferInformation-arrivalLine"></span>
                </div>
              </div>
            </div>
          </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {},
  data () {
    return {
      shiftList: [],
      selectStation: {
        departureStation: '',
        arrivalStation: '',
      },
      searchTime: {}
    }
  },
  computed: {},
  components: {},
  watch: {},
  mixins: [],
  methods: {
    toggleShowInformation(shift){
      var transferInformation = document.querySelectorAll(".treroad-searchResults-transferInformation")
      var downIcon = document.querySelectorAll('.treroad-searchResults-trainInformation-downIcon')
      var index = this.shiftList.indexOf(shift)
      console.log(transferInformation[index])
      console.log(index)
      if(downIcon[index].style.transform == 'rotate(-180deg)') {
        console.log(downIcon[index].style.transform)
        downIcon[index].style.transform = 'rotate(0deg)'
      }else{
        console.log(downIcon[index].style.transform)
        downIcon[index].style.transform = 'rotate(-180deg)'
      }
     
      if(transferInformation[index].style.display == 'block') {
        console.log(transferInformation[index].style.display)
        transferInformation[index].style.display = 'none'
      }else{
        console.log(transferInformation[index].style.display)
        transferInformation[index].style.display = 'block'
      }
    },
    getResult (changeInformation) {
      this.searchTime = this.$store.state.searchTime
      console.log(this.searchTime)
      this.selectStation.departureStation = this.$store.state.departureStation
      this.selectStation.arrivalStation = this.$store.state.arrivalStation
      var shiftList = this.$store.state.result
      this.changeInformation (shiftList)
    },
    changeInformation (shiftList) {
      console.log(shiftList)
      var shiftList = shiftList.map(shift => {
        var trainInformation = {
            trainClassification: '',
            trainNumber: '',
            departureTime: '',
            arrivalTime: '',
            travelTime: '',
            price: 0,
            transfer: 0
          }
        var transferInformationList = {
            trainClassification: ``,
            trainNumber: ``,
            departureTime: ``,
            arrivalTime: ``,
            departureStation: ``,
            arrivalStation: ``,
            travelTime: ``,
            price: 0,
          }
        var transferInformation = [] 
        
        shift.forEach((train, index) => {
          
          //-----------------------trainInformation-----------------------
          //trainOfTrainClassification
          trainInformation.trainClassification = shift[0].trainClassification

          //trainOfTrainNumber
          trainInformation.trainNumber = shift[0].trainNumber

          //departureTime
          var trainDepartureTime = shift[0].routes[0].departureTime
          var trainDepartureDate = new Date(trainDepartureTime)
          var trainDepartureHour = trainDepartureDate.getHours()
          var trainDepartureMinute = trainDepartureDate.getMinutes()
          if(trainDepartureHour < 10) trainDepartureHour = `0${trainDepartureHour}`
          if(trainDepartureMinute < 10) trainDepartureMinute = `0${trainDepartureMinute}`
          trainInformation.departureTime = `${trainDepartureHour}:${trainDepartureMinute}`

          //trainOfArrivalTime
          var endOfShift = shift[shift.length - 1]
          var trainArrivalTime = shift[shift.length - 1].routes[endOfShift.routes.length - 1].arrivalTime
          var trainArrivalDate = new Date(trainArrivalTime)
          var trainArrivalHour = trainArrivalDate.getHours()
          var trainArrivalMinute = trainArrivalDate.getMinutes()
          if(trainArrivalHour < 10) trainArrivalHour = `0${trainArrivalHour}`
          if(trainArrivalMinute < 10) trainArrivalMinute = `0${trainArrivalMinute}`
          trainInformation.arrivalTime = `${trainArrivalHour}:${trainArrivalMinute}`

          //trainOfTravelTime
          var trainTravelHours = trainArrivalHour - trainDepartureHour
          if(trainTravelHours < 0) trainTravelHours = trainTravelHours + 24
          var trainTravelMinutes = trainArrivalMinute - trainDepartureMinute

          var trainTravelTime = (trainTravelHours * 60) + trainTravelMinutes
          var trainRealTravelHours = (trainTravelTime - (trainTravelTime % 60)) / 60
          var trainRealTravelMinutes = trainTravelTime % 60
          if(trainRealTravelMinutes < 10) trainRealTravelMinutes = `0${trainRealTravelMinutes}`
          if(trainTravelTime > 60){
            trainInformation.travelTime = `${trainRealTravelHours}時${trainRealTravelMinutes}分`
          }else{
            trainInformation.travelTime = `${trainRealTravelMinutes}分`
          }

          //trainOfPrice
          trainInformation.price = shift.reduce((total, trainPrice) => {
            return total + trainPrice.price
          }, 0)

          //trainOfTransfer
          trainInformation.transfer = shift.length - 1

          
          //-----------------------transferInformation-----------------------
          transferInformation.push(Object.assign({}, transferInformationList))

          //transferOfTrainClassification
          transferInformation[index].trainClassification = shift[index].trainClassification

          //transferOfTrainNumber
          transferInformation[index].trainNumber = shift[index].trainNumber
          
          //transferOfDepartureTime
          var transferDepartureTime = shift[index].routes[0].departureTime
          var transferDepartureDate = new Date(transferDepartureTime)
          var transferDepartureHour = transferDepartureDate.getHours()
          var transferDepartureMinute = transferDepartureDate.getMinutes()
          if(transferDepartureHour < 10) transferDepartureHour = `0${transferDepartureHour}`
          if(transferDepartureMinute < 10) transferDepartureMinute = `0${transferDepartureMinute}`
          transferInformation[index].departureTime = `${transferDepartureHour}:${transferDepartureMinute}`

          //transferOfArrivalTime
          var transferArrivalTime = shift[index].routes[shift[index].routes.length - 1].arrivalTime
          var transferArrivalDate = new Date(transferArrivalTime)
          var transferArrivalHour = transferArrivalDate.getHours()
          var transferArrivalMinute = transferArrivalDate.getMinutes()
          if(transferArrivalHour < 10) transferArrivalHour = `0${transferArrivalHour}`
          if(transferArrivalMinute < 10) transferArrivalMinute = `0${transferArrivalMinute}`
          transferInformation[index].arrivalTime = `${transferArrivalHour}:${transferArrivalMinute}`
          
          //transferOfDepartureStation
          transferInformation[index].departureStation = shift[index].routes[0].departureStationName

          //transferOfArrivalStation
          transferInformation[index].arrivalStation = shift[index].routes[shift[index].routes.length - 1].arrivalStationName
          
          //transferOfTravelTime
          var transferTravelHours = transferArrivalHour - transferDepartureHour
          if(transferTravelHours < 0) transferTravelHours = transferTravelHours + 24
          var transferTravelMinutes = transferArrivalMinute - transferDepartureMinute

          var transferTravelTime = (transferTravelHours * 60) + transferTravelMinutes
          var transferRealTravelHours = (transferTravelTime - (transferTravelTime % 60)) / 60
          var transferRealTravelMinutes = transferTravelTime % 60
          if(transferRealTravelMinutes < 10) transferRealTravelMinutes = `0${transferRealTravelMinutes}`
          if(transferTravelTime > 60){
            transferInformation[index].travelTime = `${transferRealTravelHours}時${transferRealTravelMinutes}分`
          }else{
            transferInformation[index].travelTime = `${transferRealTravelMinutes}分`
          }
          
          //transferOfPrice
          transferInformation[index].price = shift[index].price
        })
        return {
          trainInformation,
          transferInformation
        }
      })
      console.log(shiftList)
      return this.shiftList = shiftList
    },
    splitDay () {
      this.searchTime.day = this.searchTime.day.split("-")
    }
  },
  // Life cycle hook
  beforeCreate () {},
  mounted () {
    this.getResult (),
    this.splitDay ()
  }
}
</script>

<style lang="sass" scoped>
  .treroad-searchResults-page
    width: 100%
    .treroad-searchResults-resultHeader
      width: 575px
      height: 110px
      margin: 0 auto
      box-sizing: border-box
      display: flex
      justify-content: space-between
      align-items: flex-end
      box-sizing: border-box
      padding-bottom: 6px
      overflow: hidden
      @media screen and (max-width: 600px)
        width: 100%
        justify-content: flex-end
        align-items: flex-start
        flex-direction: column
        padding: 19px auto 8px 8px
        height: 130px
      .treroad-searchResults-queryCondition
        color: rgb(74, 74, 74)
        width: 175px
        height: 60px
        @media screen and (max-width: 600px)
          width: 100%
          margin-left: 8px
        .treroad-searchResults-trainRoute
          font-size: 25px
        .treroad-searchResults-searchTime
          font-size: 14px
      button
        width: 120px
        height: 36px
        border-radius: 18px
        border: 0
        background: rgb(68, 199, 168)
        color: white
        font-size: 16px
        cursor: pointer
        @media screen and (max-width: 600px)
          margin-left: 8px
    .treroad-searchResults-searchResultBackground
      width: 100%
      min-height: calc(100vh - 170px)
      background: url(../assets/result-background.png)
      @media screen and (max-width: 600px)
        padding-top: 16px
      .treroad-searchResults-searchResult
        width: 575px
        height: 100%
        margin: 0 auto
        @media screen and (max-width: 600px)
          width: 100%
        .treroad-searchResults-resultFilter
          text-align: center
          padding: 16px 0
          input
            display: none
          label
            display: inline-block
            font-size: 17px
            color: rgb(74, 74, 74)
            padding: 7px 17px
            margin: 0 4px
            background: white
            border-radius: 18px
            border: 1px solid rgb(68, 199, 168)
            cursor: pointer
          input:checked + label
            color: white
            background: rgb(68, 199, 168)
          @media screen and (max-width: 600px)
            display: none
        .treroad-searchResults-searchResultList
          width: 100%
          min-height: 87px
          box-sizing: border-box
          border-radius: 6px
          box-shadow: 0px 2px 4px rgba(0, 0, 0, .2)
          overflow: hidden
          margin-bottom: 8px
          @media screen and (max-width: 600px)
            border-radius: 0
          .treroad-searchResults-trainInformation
            width: 100%
            height: 100%
            padding: 9px 0
            display: flex
            align-items: center
            box-sizing: border-box
            position: relative
            cursor: pointer
            background: white
            border-bottom: 1px solid rgb(219, 219, 219)
            @media screen and (max-width: 600px)
              padding-left: 10px
            div
              display: inline-block
              box-sizing: border-box
              .treroad-searchResults-trainInformation-title
                margin: 7px 0
                font-size: 14px
                color: rgb(74, 74, 74)
                line-height: 20px
              .treroad-searchResults-trainInformation-value
                margin-bottom: 7px
                font-size: 20px
                color: rgb(74, 74, 74)
                line-height: 28px
            .treroad-searchResults-trainInformation-trainIcon
              width: 53px
              height: 53px
              border-radius: 12px
              margin-left: 20px
              @media screen and (max-width: 600px)
                display: none
            .treroad-searchResults-trainInformation-mobileTrainLabel
              position: absolute
              top: 8px
              left: 8px
              width: 5px
              height: 70px
              background: rgb(100, 181, 246)
              @media screen and (min-width: 600px)
                display: none
            .treroad-searchResults-trainInformation-trips
              width: 160px
              height: 100%
              padding-left: 8px
            .treroad-searchResults-trainInformation-driveTime
              width: 117px
              height: 100%
              padding-left: 8px
              border-left: 1px solid rgb(216, 216, 216)
              @media screen and (max-width: 600px)
                display: none
            .treroad-searchResults-trainInformation-fare
              width: 88px
              height: 100%
              padding-left: 8px
              border-left: 1px solid rgb(216, 216, 216)
            .treroad-searchResults-trainInformation-numberOfTransfer
              width: 137px
              height: 100%
              padding-left: 8px
              border-left: 1px solid rgb(216, 216, 216)
            .treroad-searchResults-trainInformation-downIcon
              width: auto
              height: 12px
              position: absolute
              right: 36px
              transform: rotate(0deg)
              transition: all .2s linear
          .treroad-searchResults-transferInformation
            background: white
            display: none
            .treroad-searchResults-transferInformation-transferArea
              div
                display: flex
                align-items: center
                .treroad-searchResults-transferInformation-station
                  position: absolute
                  top: 19px
                  left: 92px
              .treroad-searchResults-transferInformation-departureStation, .treroad-searchResults-transferInformation-arrivalStation
                width: 100%
                height: 60px
                position: relative
                color: rgb(74, 74, 74)
                .treroad-searchResults-transferInformation-circle
                  width: 15px
                  height: 15px
                  border: 4px solid black
                  border-radius: 50%
                  position: absolute
                  box-sizing: border-box
                  top: 23px
                  left: 56px
                  z-index: 1
                .treroad-searchResults-transferInformation-departureTime, .treroad-searchResults-transferInformation-arrivalTime
                  font-size: 12px
                  margin-left: 15px
                .treroad-searchResults-transferInformation-departureLine
                  width: 5px
                  height: 23px
                  position: absolute
                  background: rgb(100, 181, 246)
                  left: 61px
                  bottom: 0
                .treroad-searchResults-transferInformation-arrivalLine
                  width: 5px
                  height: 23px
                  position: absolute
                  background: rgb(100, 181, 246)
                  left: 61px
                  top: 0

              .treroad-searchResults-transferInformation-transferStation
                width: 100%
                height: 60px
                position: relative
                color: rgb(74, 74, 74)
                .treroad-searchResults-transferInformation-circle
                  width: 15px
                  height: 15px
                  border: 4px solid black
                  border-radius: 50%
                  position: absolute
                  box-sizing: border-box
                  left: 56px
                  z-index: 1
                  &:nth-child(5)
                    top: 8px
                  &:nth-child(6)
                    top: 37px
                .treroad-searchResults-transferInformation-departureTime, .treroad-searchResults-transferInformation-arrivalTime
                  position: absolute
                  font-size: 12px
                  left: 15px
                .treroad-searchResults-transferInformation-departureTime
                  top: 38px
                .treroad-searchResults-transferInformation-arrivalTime
                  top: 9px
                .treroad-searchResults-transferInformation-transfer
                  font-size: 12px
                  margin-left: 51px
                .treroad-searchResults-transferInformation-departureLine
                  width: 5px
                  height: 10px
                  position: absolute
                  background: rgb(100, 181, 246)
                  left: 61px
                  bottom: 0
                .treroad-searchResults-transferInformation-arrivalLine
                  width: 5px
                  height: 10px
                  position: absolute
                  background: rgb(100, 181, 246)
                  left: 61px
                  top: 0

              .treroad-searchResults-transferInformation-singleTrainInformation
                width: 100%
                height: 50px
                background: rgb(239, 242, 245)
                position: relative
                img
                  width: 26px
                  height: auto
                  border-radius: 4px
                  position: absolute
                  left: 50px
                  top: calc(50% - 13px)
                  z-index: 1
                span
                  width: 70px
                  margin-right: 25px
                  font-size: 14px
                  color: rgb(74, 74, 74)
                .treroad-searchResults-transferInformation-arrivalthoughLine
                  width: 5px
                  height: 100%
                  position: absolute
                  background: rgb(100, 181, 246)
                  left: 61px
                  top: 0
                p
                  position: absolute
                  top: 16px
                  left: 92px
                
</style>
