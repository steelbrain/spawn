# Spawn

Node's Process spawning APIs beautified, lightweight with no dependencies and Typescript typings.
Supports NodeJS ESM Loader.

## Installation

```sh
npm install --save @steelbrain/spawn
# OR
yarn add @steelbrain/spawn
```

### Usage

```typescript
import { spawn, spawnFile } from '@steelbrain/spawn'

// Simple version:
spawn('ls', []).then(function(result) {
  console.log('Exit code:', result.exitCode)
  console.log('STDOUT:', result.stdout)
  console.log('STDERR:', result.stderr)
})

// Supports childProcess#spawn options in 3rd arg
spawn('ls', [], {
  cwd: __dirname,
  stdio: 'inherit',
  windowsHide: true,
}).then(...)

// Advanced version
spawn('ls', [__dirname], {
  handleChildProcess(childProcess) {
    // Do whatever you want to child process instance here
  },
  handleStdout(chunk) {
    // Handle stdout chunk
  },
  handleStderr(chunk) {
    // Handle stderr chunk
  },
  encoding: 'utf8',
  encoding: 'buffer',
  // ^ Setting encoding to "buffer" or null gives you
  // a buffer in handleStd{out,err} and in promise values
}).then(...)

// Invoke a JS file
spawnFile('./helloworld.js', []).then(...)
```

#### API

```js
import { SpawnOptions, ChildProcess } from 'child_process'
// ^ This import is just for reference, to tell you where the types originally
// come from. You don't need to actually import it in your project.

interface ExtendedSpawnOptions<OutputType> extends SpawnOptions {
  handleChildProcess?: (childProcess: ChildProcess) => void
  handleStdout?: (chunk: OutputType) => void
  handleStderr?: (chunk: OutputType) => void
}

interface ProcessPromise<T = any> extends Promise<T> {
  kill(signal?: NodeJS.Signals | number): boolean
}

// Different input types to "spawn" and their outputs
export function spawn(
  command: string,
  args: string[],
  options: { encoding: 'buffer' | null } & Omit<ExtendedSpawnOptions<Buffer>, 'stdio'>,
): ProcessPromise<{
  stdout: Buffer
  stderr: Buffer
  exitCode: number
}>
export function spawn(
  command: string,
  args: string[],
  options: { encoding: 'buffer' | null } & ExtendedSpawnOptions<Buffer>,
): ProcessPromise<{
  stdout: Buffer | null
  stderr: Buffer | null
  exitCode: number
}>
export function spawn(
  command: string,
  args: string[],
  options?: { encoding?: BufferEncoding } & Omit<ExtendedSpawnOptions<string>, 'stdio'>,
): ProcessPromise<{
  stdout: string
  stderr: string
  exitCode: number
}>
export function spawn(
  command: string,
  args: string[],
  options?: { encoding?: BufferEncoding } & ExtendedSpawnOptions<string>,
): ProcessPromise<{
  stdout: string | null
  stderr: string | null
  exitCode: number
}>

// Different input types to "spawnFile" and their outputs
export function spawnFile(
  filePath: string,
  args: string[],
  options: { encoding: 'buffer' | null } & Omit<ExtendedSpawnOptions<Buffer>, 'stdio'>,
): ProcessPromise<{
  stdout: Buffer
  stderr: Buffer
  exitCode: number
}>
export function spawnFile(
  filePath: string,
  args: string[],
  options: { encoding: 'buffer' | null } & ExtendedSpawnOptions<Buffer>,
): ProcessPromise<{
  stdout: Buffer | null
  stderr: Buffer | null
  exitCode: number
}>
export function spawnFile(
  filePath: string,
  args: string[],
  options?: { encoding?: BufferEncoding } & Omit<ExtendedSpawnOptions<string>, 'stdio'>,
): ProcessPromise<{
  stdout: string
  stderr: string
  exitCode: number
}>
export function spawnFile(
  filePath: string,
  args: string[],
  options?: { encoding?: BufferEncoding } & ExtendedSpawnOptions<string>,
): ProcessPromise<{
  stdout: string | null
  stderr: string | null
  exitCode: number
}>
```

#### License

This project is licensed under the terms of MIT License. See the License file for more info.
