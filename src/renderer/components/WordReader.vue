<template>
    <div>
        <button @click="ChooseFile">Choose File</button>
        <button @click="ReadLine(10)">Read File</button>
    </div>
</template>

<script>
import fs from 'fs'
import readline from 'readline'
import { remote } from 'electron'

export default {
  data () {
    return {
      lineCounter: 0,
      filepath: ''
    }
  },
  methods: {
    ChooseFile () {
      var vm = this
      remote.dialog.showOpenDialog({properties: ['openFile'], filters: [{name: 'Wordlist', extensions: ['txt']}]}, (filePaths) => {
        if (filePaths == null) {
          console.log(`Read nothing`)
          return
        }
        vm.filepath = filePaths[0]
        console.log(`File read: ${vm.filepath}`)
      })
    },
    ReadLine (index) {
      var vm = this
      if (vm.filepath === '' || vm.filepath === null) {
        console.log('no file opened')
        return
      }
      var instream = fs.createReadStream(vm.filepath)
      var rl = readline.createInterface(instream)
      rl.on('line', (line) => {
        if (vm.lineCounter++ === index) {
          console.log(`Line read: ${line}`)
        }
      })
      rl.on('close', () => {
        vm.lineCounter = 0
        console.log('done reading')
      })
    }
  }
}
</script>