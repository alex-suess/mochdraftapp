<script setup>
import {reactive} from 'vue'


function move(direction) {
  if (direction === 'left') {
    if (state.currentPick === 1) {
      return
    }
    state.currentPick--
    setCurrentTeam()
  }

  if (direction === 'right') {

    if (state.currentPick === state.picks.length) {
      return
    }

    state.currentPick++
    setCurrentTeam()
  }
  reset()
}

function toggleResults() {
  state.showResults = !state.showResults
  state.showPickOption = !state.showPickOption
  state.showTradeOption = !state.showTradeOption
}

function reset() {
  state.showPickOption = true
  state.showTradeOption = true
  state.filteredPlayerOptions = []
  state.playerInput = ''
  state.tradePicks = []
  state.tradeTeam = ''
  state.filteredTradeTeamOptions = {}
  state.tradeTeamInput = ''
}

function displayPlayerOptions() {
  state.showPickOption = true
  state.showTradeOption = false
  // filtered by input, then sorted by id (later adp), then take the first 5 elements
  state.filteredPlayerOptions = state.playerOptions.filter(player => player.name.toUpperCase().includes(state.playerInput.toUpperCase())).sort((a, b) => a.id - b.id)
}

function displayTradeTeamOptions() {
  state.showPickOption = false
  state.showTradeOption = true

  // first remove the current team from trade partner options
  state.filteredTradeTeamOptions = state.teams.filter(team => team.name !== state.currentTeam.name && team.name.toUpperCase().includes(state.tradeTeamInput.toUpperCase()))
}

function hidePlayerOptions() {
  state.playerInput = ''
  state.showPickOption = true
  state.showTradeOption = true
  state.filteredPlayerOptions = []
}

function submitPick(playerId) {
  if (state.results[state.currentPick]) {
    state.playerOptions.push(state.results[state.currentPick])
  }
  state.results[state.currentPick] = state.playerOptions.filter(player => player.id === playerId)[0]

  removePlayerFromOptions(playerId)
  hidePlayerOptions()
  if (state.currentPick === state.picks.length) {
    toggleResults()
    return
  }
  move('right')
}

function removePlayerFromOptions(playerId) {
  state.playerOptions = state.playerOptions.filter(player => player.id !== playerId)
}

function tradePicks() {
  state.tradePicks.forEach(function (pickNumber) {
    state.picks = state.picks.map(pick => {
      if (pick.pickNumber === pickNumber) {
        return {...pick, team: state.currentTeam.name};
      }
      return pick;
    })
  })


  state.picks = state.picks.map(pick => {
    if (pick.pickNumber === state.currentPick) {
      return {...pick, team: state.tradeTeam};
    }
    return pick;
  })

  setCurrentTeam()
  reset()
}

function setCurrentTeam() {
  const currentTeamName = state.picks.filter(pick => pick.pickNumber === state.currentPick)[0].team
  state.currentTeam = state.teams.filter(team => team.name === currentTeamName)[0]
}

const state = reactive({
  showResults: false,
  showTradeOption: true,
  showPickOption: true,
  showTradeInputOptions: false,
  playerInput: '',
  tradeTeamInput: '',
  playerOptions: [
    {'id': 1, 'name': 'Will Anderson Jr.', 'school': 'Alabama', 'year': 'Jr', 'position': 'EDGE'},
    {'id': 2, 'name': 'Bryce Young', 'school': 'Alabama', 'year': 'Jr', 'position': 'QB'},
    {'id': 3, 'name': 'Jalen Carter', 'school': 'Georgia', 'year': 'Jr', 'position': 'DL'},
    {'id': 4, 'name': 'C.J. Stroud 	Ohio', 'school': 'State', 'year': 'Jr', 'position': 'QB'},
    {'id': 5, 'name': 'Bijan Robinson', 'school': 'Texas', 'year': 'Jr', 'position': 'RB'},
    {'id': 6, 'name': 'Peter Skoronski', 'school': 'Northwestern', 'year': 'Jr', 'position': 'OT'},
    {'id': 7, 'name': 'Christian Gonzalez', 'school': 'Oregon', 'year': 'Soph', 'position': 'CB'},
    {'id': 8, 'name': 'Tyree Wilson 	Texas', 'school': 'Tech', 'year': 'Sr', 'position': 'EDGE'},
    {'id': 9, 'name': 'Devon Witherspoon', 'school': 'Illinois', 'year': 'Jr', 'position': 'CB'},
    {'id': 10, 'name': 'Deonte Banks', 'school': 'Maryland', 'year': 'Jr', 'position': 'CB'},
    {'id': 11, 'name': 'Anthony Richardson', 'school': 'Florida', 'year': 'Soph', 'position': 'QB'},
    {'id': 12, 'name': 'Joey Porter Jr.', 'school': 'Penn State', 'year': 'Jr', 'position': 'CB'},
    {'id': 13, 'name': 'Paris Johnson Jr.', 'school': 'Ohio State', 'year': 'Jr', 'position': 'OT'},
    {'id': 14, 'name': 'Jahmyr Gibbs', 'school': 'Alabama', 'year': 'Jr', 'position': 'RB'},
    {'id': 15, 'name': 'Quentin Johnston', 'school': 'TCU', 'year': 'Jr', 'position': 'WR'},
    {'id': 16, 'name': 'Lukas Van Ness', 'school': 'Iowa', 'year': 'Soph', 'position': 'EDGE'},
    {'id': 17, 'name': 'Brian Branch', 'school': 'Alabama', 'year': 'Jr', 'position': 'S'},
    {'id': 18, 'name': 'Will Levis', 'school': 'Kentucky', 'year': 'Sr', 'position': 'QB'},
    {'id': 19, 'name': 'Jaxon Smith-Njigba', 'school': 'Ohio State', 'year': 'Jr', 'position': 'WR'},
    {'id': 20, 'name': 'Myles Murphy', 'school': 'Clemson', 'year': 'Jr', 'position': 'EDGE'},
    {'id': 21, 'name': 'Michael Mayer', 'school': 'Notre Dame', 'year': 'Jr', 'position': 'TE'},
    {'id': 22, 'name': 'Nolan Smith', 'school': 'Georgia', 'year': 'Sr', 'position': 'EDGE'},
    {'id': 23, 'name': 'Broderick Jones', 'school': 'Georgia', 'year': 'Soph', 'position': 'OT'},
    {'id': 24, 'name': 'Zay Flowers', 'school': 'Boston College', 'year': 'Sr', 'position': 'WR'},
    {'id': 25, 'name': 'Jordan Addison', 'school': 'USC', 'year': 'Jr', 'position': 'WR'},
    {'id': 26, 'name': 'Darnell Wright', 'school': 'Tennessee', 'year': 'Sr', 'position': 'OT'},
    {'id': 27, 'name': 'Bryan Bresee', 'school': 'Clemson', 'year': 'Soph', 'position': 'DL'},
    {'id': 28, 'name': 'Anton Harrison', 'school': 'Oklahoma', 'year': 'Jr', 'position': 'OT'},
    {'id': 29, 'name': 'Drew Sanders', 'school': 'Arkansas', 'year': 'Jr', 'position': 'LB'},
    {'id': 30, 'name': 'Calijah Kancey', 'school': 'Pittsburgh', 'year': 'Jr', 'position': 'DL'},
    {'id': 31, 'name': 'Dalton Kincaid', 'school': 'Utah', 'year': 'Sr', 'position': 'TE'},
    {'id': 32, 'name': 'Cam Smith', 'school': 'South Carolina', 'year': 'Jr', 'position': 'CB'},
    {'id': 33, 'name': 'O\'Cyrus Torrence', 'school': 'Florida', 'year': 'Jr', 'position': 'IOL'},
    {'id': 34, 'name': 'Emmanuel Forbes ', 'school': 'Miss. State', 'year': 'Jr', 'position': 'CB'},
    {'id': 35, 'name': 'Darnell Washington', 'school': 'Georgia', 'year': 'Jr', 'position': 'TE'},
    {'id': 36, 'name': 'Steve Avila', 'school': 'TCU', 'year': 'Sr', 'position': 'IOL'},
    {'id': 37, 'name': 'John Michael Schmitz', 'school': 'Minnesota', 'year': 'Sr', 'position': 'IOL'},
    {'id': 38, 'name': 'Adetomiwa Adebawore', 'school': 'Northwestern', 'year': 'Sr', 'position': 'DL'},
    {'id': 39, 'name': 'Derick Hall', 'school': 'Auburn', 'year': 'Sr', 'position': 'EDGE'},
    {'id': 40, 'name': 'Jack Campbell', 'school': 'Iowa', 'year': 'Sr', 'position': 'LB'},
    {'id': 41, 'name': 'Sam LaPorta', 'school': 'Iowa', 'year': 'Sr', 'position': 'TE'},
    {'id': 42, 'name': 'Keion White', 'school': 'Georgia Tech', 'year': 'Sr', 'position': 'EDGE'},
    {'id': 43, 'name': 'BJ Ojulari', 'school': 'LSU', 'year': 'Jr', 'position': 'EDGE'},
    {'id': 44, 'name': 'Cody Mauch', 'school': 'N. Dakota St.', 'year': 'Sr', 'position': 'OT'},
    {'id': 45, 'name': 'Matthew Bergeron', 'school': 'Syracuse', 'year': 'Jr', 'position': 'OT'},
    {'id': 46, 'name': 'Josh Downs', 'school': 'North Carolina', 'year': 'Jr', 'position': 'WR'},
    {'id': 47, 'name': 'Daiyan Henley ', 'school': 'Washington St.', 'year': 'Sr', 'position': 'LB'},
    {'id': 48, 'name': 'Kelee Ringo', 'school': 'Georgia', 'year': 'Soph', 'position': 'CB'},
    {'id': 49, 'name': 'Felix Anudike-Uzomah ', 'school': 'Kansas State', 'year': 'Jr', 'position': 'EDGE'},
    {'id': 50, 'name': 'Mazi Smith', 'school': 'Michigan', 'year': 'Sr', 'position': 'DL'},
    {'id': 51, 'name': 'Tyler Scott', 'school': 'Cincinnati', 'year': 'Jr', 'position': 'WR'},
    {'id': 52, 'name': 'Will McDonald IV ', 'school': 'Iowa St.', 'year': 'Sr', 'position': 'EDGE'},
    {'id': 53, 'name': 'Joe Tippmann', 'school': 'Wisconsin', 'year': 'Jr', 'position': 'IOL'},
    {'id': 54, 'name': 'Jakorian Bennett', 'school': 'Maryland', 'year': 'Sr', 'position': 'CB'},
    {'id': 55, 'name': 'Hendon Hooker', 'school': 'Tennessee', 'year': 'Sr', 'position': 'QB'},
    {'id': 56, 'name': 'Trenton Simpson', 'school': 'Clemson', 'year': 'Jr', 'position': 'LB'},
    {'id': 57, 'name': 'JL Skinner', 'school': 'Boise St.', 'year': 'Sr', 'position': 'S'},
    {'id': 58, 'name': 'Luke Musgrave', 'school': 'Oregon State', 'year': 'Jr', 'position': 'TE'},
    {'id': 59, 'name': 'Jalen Redmond', 'school': 'Oklahoma', 'year': 'Jr', 'position': 'EDGE'},
    {'id': 60, 'name': 'Tuli Tuipulotu', 'school': 'USC', 'year': 'Jr', 'position': 'DL'},
    {'id': 61, 'name': 'Jalin Hyatt', 'school': 'Tennessee', 'year': 'Jr', 'position': 'WR'},
    {'id': 62, 'name': 'Isaiah Foskey', 'school': 'Notre Dame', 'year': 'Jr', 'position': 'EDGE'},
    {'id': 63, 'name': 'Jartavius Martin', 'school': 'Illinois', 'year': 'Sr', 'position': 'S'},
    {'id': 64, 'name': 'DJ Turner', 'school': 'Michigan', 'year': 'Sr', 'position': 'CB'},
    {'id': 65, 'name': 'Julius Brents', 'school': 'Kansa State', 'year': 'Sr', 'position': 'CB'},
    {'id': 66, 'name': 'Blake Freeland', 'school': 'BYU', 'year': 'Jr', 'position': 'OT'},
    {'id': 67, 'name': 'Karl Brooks', 'school': 'Bowling Green', 'year': 'Sr', 'position': 'DL'},
    {'id': 68, 'name': 'Andre Carter II', 'school': 'Army', 'year': 'Sr', 'position': 'EDGE'},
    {'id': 69, 'name': 'Gervon Dexter Sr.', 'school': 'Florida', 'year': 'Soph', 'position': 'DL'},
    {'id': 70, 'name': 'Antonio Johnson ', 'school': 'Texas A&M', 'year': 'Jr', 'position': 'S'},
    {'id': 71, 'name': 'Byron Young', 'school': 'Tennessee', 'year': 'Sr', 'position': 'EDGE'},
    {'id': 72, 'name': 'Jayden Reed', 'school': 'Michigan St.', 'year': 'Sr', 'position': 'WR'},
    {'id': 73, 'name': 'Marte Mapu', 'school': 'Sacramento St.', 'year': 'Sr', 'position': 'S'},
    {'id': 74, 'name': 'Rashee Rice', 'school': 'SMU', 'year': 'Sr', 'position': 'WR'},
    {'id': 75, 'name': 'Byron Young', 'school': 'Alabama', 'year': 'Sr', 'position': 'DL'},
    {'id': 76, 'name': 'Marvin Mims', 'school': 'Oklahoma', 'year': 'Jr', 'position': 'WR'},
    {'id': 77, 'name': 'Cedric Tillman', 'school': 'Tennessee', 'year': 'Sr', 'position': 'WR'},
    {'id': 78, 'name': 'Zacch Pickens', 'school': 'South Carolina', 'year': 'Sr', 'position': 'DL'},
    {'id': 79, 'name': 'Roschon Johnson', 'school': 'Texas', 'year': 'Sr', 'position': 'RB'},
    {'id': 80, 'name': 'Nick Saldiveri', 'school': 'Old Dominion', 'year': 'Jr', 'position': 'IOL'},
    {'id': 81, 'name': 'Moro Ojomo', 'school': 'Texas', 'year': 'Sr', 'position': 'DL'},
    {'id': 82, 'name': 'Clark Phillips III', 'school': 'Utah', 'year': 'Soph', 'position': 'CB'},
    {'id': 83, 'name': 'Ji\'Ayir Brown', 'school': 'Penn State', 'year': 'Sr', 'position': 'CB'},
    {'id': 84, 'name': 'Darius Rush', 'school': 'South Carolina', 'year': 'Sr', 'position': 'CB'},
    {'id': 85, 'name': 'Cory Trice', 'school': 'Purdue', 'year': 'Sr', 'position': 'CB'},
    {'id': 86, 'name': 'Devon Achane', 'school': 'Texas A&M', 'year': 'Jr', 'position': 'RB'},
    {'id': 87, 'name': 'Keeanu Benton', 'school': 'Wisconsin', 'year': 'Sr', 'position': 'DL'},
    {'id': 88, 'name': 'Chandler Zavala ', 'school': 'NC State', 'year': 'Sr', 'position': 'IOL'},
    {'id': 89, 'name': 'Dawand Jones', 'school': 'Ohio State', 'year': 'Sr', 'position': 'OT'},
    {'id': 90, 'name': 'Tyjae Spears', 'school': 'Tulane', 'year': 'Jr', 'position': 'RB'},
    {'id': 91, 'name': 'Sydney Brown', 'school': 'Illinois', 'year': 'Sr', 'position': 'S'},
    {'id': 92, 'name': 'YaYa Diaby', 'school': 'Louisville', 'year': 'Sr', 'position': 'EDGE'},
    {'id': 93, 'name': 'Eli Ricks', 'school': 'Alabama', 'year': 'Jr', 'position': 'CB'},
    {'id': 94, 'name': 'Zach Harrison', 'school': 'Ohio State', 'year': 'Sr', 'position': 'EDGE'},
    {'id': 95, 'name': 'Jordan Battle', 'school': 'Alabama', 'year': 'Sr', 'position': 'S'},
    {'id': 96, 'name': 'Colby Wooden', 'school': 'Auburn', 'year': 'Jr', 'position': 'DL'},
    {'id': 97, 'name': 'Parker Washington', 'school': 'Penn State', 'year': 'Soph', 'position': 'WR'},
    {'id': 98, 'name': 'Tucker Kraft', 'school': 'S. Dakota St.', 'year': 'Jr', 'position': 'TE'},
    {'id': 99, 'name': 'Nathaniel Dell', 'school': 'Houston', 'year': 'Jr', 'position': 'WR'},
    {'id': 100, 'name': 'Jaquelin Roy', 'school': 'LSU', 'year': 'Jr', 'position': 'DL'},
  ],
  picks: [
    {'pickNumber': 1, 'team': 'CAR'},
    {'pickNumber': 2, 'team': 'HOU'},
    {'pickNumber': 3, 'team': 'ARI'},
    {'pickNumber': 4, 'team': 'IND'},
    {'pickNumber': 5, 'team': 'SEA'},
    {'pickNumber': 6, 'team': 'DET'},
    {'pickNumber': 7, 'team': 'LV'},
    {'pickNumber': 8, 'team': 'ATL'},
    {'pickNumber': 9, 'team': 'CHI'},
    {'pickNumber': 10, 'team': 'PHI'},
    {'pickNumber': 11, 'team': 'TEN'},
    {'pickNumber': 12, 'team': 'HOU'},
    {'pickNumber': 13, 'team': 'GB'},
    {'pickNumber': 14, 'team': 'NE'},
    {'pickNumber': 15, 'team': 'NYJ'},
    {'pickNumber': 16, 'team': 'WAS'},
    {'pickNumber': 17, 'team': 'PIT'},
    {'pickNumber': 18, 'team': 'DET'},
    {'pickNumber': 19, 'team': 'TB'},
    {'pickNumber': 20, 'team': 'SEA'},
    {'pickNumber': 21, 'team': 'LAC'},
    {'pickNumber': 22, 'team': 'BAL'},
    {'pickNumber': 23, 'team': 'MIN'},
    {'pickNumber': 24, 'team': 'JAX'},
    {'pickNumber': 25, 'team': 'NYG'},
    {'pickNumber': 26, 'team': 'DAL'},
    {'pickNumber': 27, 'team': 'BUF'},
    {'pickNumber': 28, 'team': 'CIN'},
    {'pickNumber': 29, 'team': 'NO'},
    {'pickNumber': 30, 'team': 'PHI'},
    {'pickNumber': 31, 'team': 'KC'}
  ],
  results: {
    1: null,
    2: null,
    3: null
  },
  currentPick: 1,
  filteredPlayerOptions: [],
  currentTeam: {},
  teams: [
    {'color': 'bg-sky-300', 'name': 'CAR'},
    {'color': 'bg-blue-900', 'name': 'HOU'},
    {'color': 'bg-red-600', 'name': 'ARI'},
    {'color': 'bg-blue-300', 'name': 'IND'},
    {'color': 'bg-sky-700', 'name': 'SEA'},
    {'color': 'bg-blue-400', 'name': 'DET'},
    {'color': 'bg-black', 'name': 'LV'},
    {'color': 'bg-red-700', 'name': 'ATL'},
    {'color': 'bg-amber-800', 'name': 'CHI'},
    {'color': 'bg-lime-700', 'name': 'PHI'},
    {'color': 'bg-slate-300', 'name': 'TEN'},
    {'color': 'bg-orange-400', 'name': 'DEN'},
    {'color': 'bg-green-600', 'name': 'GB'},
    {'color': 'bg-blue-800', 'name': 'NE'},
    {'color': 'bg-lime-800', 'name': 'NYJ'},
    {'color': 'bg-red-800', 'name': 'WAS'},
    {'color': 'bg-gray-800', 'name': 'PIT'},
    // {'color': 18, 'name': 'DET'},
    {'color': 'bg-orange-700', 'name': 'TB'},
    // {'color': 20, 'name': 'SEA'},
    {'color': 'bg-sky-500', 'name': 'LAC'},
    {'color': 'bg-purple-800', 'name': 'BAL'},
    {'color': 'bg-purple-600', 'name': 'MIN'},
    {'color': 'bg-cyan-500', 'name': 'JAX'},
    {'color': 'bg-sky-600', 'name': 'NYG'},
    {'color': 'bg-slate-400', 'name': 'DAL'},
    {'color': 'bg-red-500', 'name': 'BUF'},
    {'color': 'bg-orange-500', 'name': 'CIN'},
    {'color': 'bg-gray-900', 'name': 'NO'},
    //{'color': 30, 'name': 'PHI'},
    {'color': 'bg-red-600', 'name': 'KC'},
    {'color': 'bg-cyan-500', 'name': 'MIA'},
  ],
  tradeTeam: '',
  filteredTradeTeamOptions: {},
  tradePicks: []
})
if (window.innerWidth > 1000) {
  state.showResults = true
}
setCurrentTeam()
</script>

<template>
  <div class="h-screen flex flex-col">
    <header>
      <div class="" :class="state.currentTeam.color">
        <div class="flex text-white max-w-4xl mx-auto px-12 lg:px-0">
          <button @click="move('left')" class="w-12 flex items-center opacity-50 cursor-pointer" :class="{
                      'opacity-0 cursor-default':  state.currentPick === 1
                  }">
            <svg aria-hidden="true" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24"
                 xmlns="http://www.w3.org/2000/svg">
              <path d="M15.75 19.5L8.25 12l7.5-7.5" stroke-linecap="round" stroke-linejoin="round"></path>
            </svg>
          </button>
          <div class="text-center font-bold py-8 flex-1">
            <h2 class="text-5xl">Pick {{ state.currentPick + ': ' + state.currentTeam.name }}</h2>
            <h5 class="text-sm opacity-70" v-if="state.results[state.currentPick]">current pick:
              {{ state.results[state.currentPick].name }}</h5>
          </div>
          <button @click="move('right')" class="w-12 flex items-center opacity-50 cursor-pointer" :class="{
                      'opacity-0 cursor-default':  state.currentPick === state.picks.length
                  }">
            <svg aria-hidden="true" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24"
                 xmlns="http://www.w3.org/2000/svg">
              <path d="M8.25 4.5l7.5 7.5-7.5 7.5" stroke-linecap="round" stroke-li nejoin="round"></path>
            </svg>
          </button>
        </div>
      </div>
    </header>
    <div class="flex-1 flex flex-col max-w-4xl mx-auto w-full">
      <div class="flex justify-end py-4 text-sky-900 px-12 lg:px-0" v-if="!state.showResults">
        <button class="hover:underline" @click="toggleResults()">View Results</button>
      </div>
      <div class="flex items-center h-full gap-12">
        <div v-if="state.showTradeOption || state.showPickOption" class="flex flex-col flex-1 h-full">
          <div v-if="state.showPickOption" class="flex-1 flex flex-col items-center justify-center">
            <div class="w-full">
              <div class="pb-8 lg:px-0 px-12">
                <input type="text"
                       class="shadow-lg w-full bg-sky-100 rounded-xl py-8 px-6 text-xl placeholder:text-sky-800 placeholder:tracking-wider placeholder:font-semibold placeholder:text-center"
                       placeholder="SELECT A PLAYER" v-model="state.playerInput"
                       @input="displayPlayerOptions"
                       @click="displayPlayerOptions">
              </div>


              <div v-if="state.filteredPlayerOptions.length" class="pb-8 player-select px-12 lg:px-0">
                <div v-for="player in state.filteredPlayerOptions"
                     class="hover:bg-sky-100 cursor-pointer text-xl flex items-center p-2 rounded-lg">
                <span class="p-2 w-16 rounded-lg text-center text-base font-bold text-white" :class="{
                  'bg-red-400': player.position=='QB',
                  'bg-blue-400': player.position=='RB',
                  'bg-yellow-400': player.position == 'TE',
                  'bg-green-400': player.position == 'WR',
                  'bg-pink-400': player.position == 'DT',
                  'bg-purple-400': player.position == 'LB',
                  'bg-gray-400': player.position == 'S',
                  'bg-orange-600': player.position === 'OT',
                  'bg-lime-700': player.position === 'EDGE',
                  'bg-teal-500': player.position === 'CB',
                  'bg-teal-300': player.position === 'IOL',
                  'bg-fuchsia-300': player.position === 'DL',
                }">
                  {{ player.position }}
                </span>
                  <div class="px-4 py-2 flex items-center gap-4" @click="submitPick(player.id)">
                    {{ player.name }} <span class="text-gray-400 uppercase text-sm">{{player.school}}</span>
                  </div>
                </div>
              </div>
              <div class="px-12 lg:px-0">
                <button v-if="!state.showTradeOption" class="text-sky-900 text-center w-full pt-8 text-lg"
                        @click="reset">Go Back
                </button>
              </div>
            </div>
          </div>
          <div v-if="state.showTradeOption && state.showPickOption" class="flex items-center px-32">
            <div class="border flex-1"></div>
            <div class="px-4 text-gray-400 font-semibold">OR</div>
            <div class="border flex-1"></div>
          </div>
          <div v-if="state.showTradeOption" class="flex-1 px-12 lg:px-0 flex flex-col items-center justify-center">
            <div class="w-full">
              <input v-if="!state.tradeTeam" placeholder="TRADE THIS PICK"
                     class="mt-4 shadow-lg w-full bg-orange-100 rounded-xl py-8 px-6 text-xl placeholder:text-orange-800 placeholder:tracking-wider placeholder:font-semibold placeholder:text-center"
                     @click='displayTradeTeamOptions' @input="displayTradeTeamOptions" type="text"
                     v-model="state.tradeTeamInput">
              <div v-if="state.tradeTeam"
                   class="mt-4 w-full py-4 px-6 text-xl text-orange-800 tracking-wider font-semibold text-center uppercase">
                Trade with {{ state.tradeTeam }}
              </div>
              <div v-if="state.showTradeOption && !state.showPickOption">
                <div class="mt-8">
                  <div v-if="state.filteredTradeTeamOptions.length && !state.tradeTeam">
                    <div class="text-gray-600 pb-2">1. Select a team to trade with</div>
                    <div class="hover:bg-sky-100 cursor-pointer text-xl flex items-center p-2 rounded-lg"
                         @click="state.tradeTeam = team.name"
                         v-for="team in state.filteredTradeTeamOptions">
                      {{ team.name }} <span class="ml-4 text-gray-400">Picks {{
                        state.picks.map(pick => pick.team === team.name ? '#' + pick.pickNumber : '').join(' ')
                      }}</span>
                    </div>
                  </div>
                </div>
                <div class="" v-if="state.tradeTeam">
                  <div class="text-gray-600 pb-2">2. Select the picks the team gives in return. If a resource outside of
                    this mockdraft is traded you can just click "Execute Trade"
                  </div>
                  <div class="flex gap-4 items-center">
                    <button @click="state.tradePicks.push(pick.pickNumber)"
                            class="py-4 bg-orange-100 mt-2 rounded-lg w-32 hover:bg-sky-300" :class="{
                          'bg-sky-300': state.tradePicks.includes(pick.pickNumber)
                        }" v-for="pick in state.picks.filter(pick => pick.team === state.tradeTeam)">
                      Pick #{{ pick.pickNumber }}
                    </button>
                  </div>
                </div>
                <div class="mt-12" v-if="state.tradeTeam">
                  <button @click="tradePicks()"
                          class="py-4 bg-sky-800 text-white mt-2 rounded-lg w-32 hover:bg-sky-700">Execute Trade
                  </button>
                </div>
                <div class="px-12 lg:px-0">
                  <button class="text-sky-900 text-center w-full pt-8 text-lg"
                          @click="reset">Go Back
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div v-if="state.showResults" class="flex-1 flex flex-col h-full px-12 lg:px-0">
          <div class="flex justify-end py-4 text-sky-900 lg:px-0">
            <button v-if="!state.showPickOption" class="hover:underline" @click="toggleResults">Back to the Draft
            </button>
          </div>
          <div class="flex flex-col flex-1 text-lg">
            <div>
              <div v-for="(result, index) in state.results" class="grid grid-cols-8">
                <div class="w-8 col-span-1">{{ index }}</div>
                <div v-if="state.picks[index-1]" class="col-span-2">{{ state.picks[index - 1].team }}</div>
                <div v-if="result" class="col-span-1">{{ result.position }}</div>
                <div v-if="result" class="col-span-4">
                  {{ result.name }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.player-select {
  max-height: 220px;
  overflow-y: auto;
}
</style>