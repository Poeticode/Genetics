<template>
    <div>
        <button @click="ChooseFile">Choose File</button>
        <button @click="ReadLine(10)">Read File</button>
    </div>
</template>

<script>
import fs from 'fs-extra'
import path from 'path'
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
      var dataPath = path.join(remote.app.getPath('appData'), 'Genetics/english.txt')
      if (vm.filepath === '' || vm.filepath === null) {
        // time to read the default english dict
        vm.filepath = dataPath
        var dataReadStream = fs.createReadStream(dataPath)

        dataReadStream.on('error', (readError) => {
          // We must not have the file!
          fs.copy(path.join(__static, 'english.txt'), dataPath).then(() => {
            console.log('copied over the english dict')
            vm.ReadLine(index)
          }).catch(err => console.error(err))
        })
        dataReadStream.on('readable', () => {
          // we have the file!
          let done = false
          while (!done) {
            if (dataReadStream.read() == null) {
              done = true
            }
          }
        })
        dataReadStream.on('end', () => {
          vm.ReadLine(index)
        })
      } else {
        var instream = fs.createReadStream(vm.filepath)
        var rl = readline.createInterface(instream)
        console.log('we have the file')
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
}
</script>