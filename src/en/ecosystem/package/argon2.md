---
editLink: true
---

# @ohos-rs/argon2

- `@ohos-rs/argon2` supports all three algorithms:

    - Argon2i: Optimizes against GPU cracking attacks but vulnerable to side-channels.
      Accesses the memory array in a password dependent order, reducing the possibility of time–memory tradeoff (TMTO) attacks.
    - Argon2d: Optimized to resist side-channel attacks.
      Accesses the memory array in a password independent order, increasing the possibility of time-memory tradeoff (TMTO) attacks.
    - **Argon2id**: default value, this is the default algorithm for normative recommendations.
      Hybrid that mixes Argon2i and Argon2d passes.
      Uses the Argon2i approach for the first half pass over memory and Argon2d approach for subsequent passes. This effectively places it in the “middle” between the other two: it doesn’t provide as good TMTO/GPU cracking resistance as Argon2d, nor as good of side-channel resistance as Argon2i, but overall provides the most well-rounded approach to both classes of attacks.

## Install

```shell
ohpm install @ohos-rs/argon2
```

## Api

```ts
import buffer from '@ohos.buffer';
export const enum Algorithm {
    /**
     * Optimizes against GPU cracking attacks but vulnerable to side-channels.
     * Accesses the memory array in a password dependent order, reducing the possibility of time–memory tradeoff (TMTO) attacks.
     */
    Argon2d = 0,
    /**
     * Optimized to resist side-channel attacks.
     * Accesses the memory array in a password independent order, increasing the possibility of time-memory tradeoff (TMTO) attacks.
     */
    Argon2i = 1,
    /**
     * Default value, this is the default algorithm for normative recommendations.
     * Hybrid that mixes Argon2i and Argon2d passes.
     * Uses the Argon2i approach for the first half pass over memory and Argon2d approach for subsequent passes. This effectively places it in the “middle” between the other two: it doesn’t provide as good TMTO/GPU cracking resistance as Argon2d, nor as good of side-channel resistance as Argon2i, but overall provides the most well-rounded approach to both classes of attacks.
     */
    Argon2id = 2
}
export interface Options {
    /**
     * The amount of memory to be used by the hash function, in kilobytes. Each thread will have a memory pool of this size. Note that large values for highly concurrent usage will cause starvation and thrashing if your system memory gets full.
     *
     * Value is an integer in decimal (1 to 10 digits), between 1 and (2^32)-1.
     *
     * The default value is 4096, meaning a pool of 4 MiB per thread.
     */
    memoryCost?: number
    /**
     * The time cost is the amount of passes (iterations) used by the hash function. It increases hash strength at the cost of time required to compute.
     *
     * Value is an integer in decimal (1 to 10 digits), between 1 and (2^32)-1.
     *
     * The default value is 3.
     */
    timeCost?: number
    /**
     * The hash length is the length of the hash function output in bytes. Note that the resulting hash is encoded with Base 64, so the digest will be ~1/3 longer.
     *
     * The default value is 32, which produces raw hashes of 32 bytes or digests of 43 characters.
     */
    outputLen?: number
    /**
     * The amount of threads to compute the hash on. Each thread has a memory pool with memoryCost size. Note that changing it also changes the resulting hash.
     *
     * Value is an integer in decimal (1 to 3 digits), between 1 and 255.
     *
     * The default value is 1, meaning a single thread is used.
     */
    parallelism?: number
    algorithm?: Algorithm
    version?: Version
    secret?: buffer.Buffer
    salt?: buffer.Buffer
}
export const enum Version {
    /** Version 16 (0x10 in hex) */
    V0x10 = 0,
    /**
     * Default value
     * Version 19 (0x13 in hex)
     */
    V0x13 = 1
}
export function hash(password: string | buffer.Buffer, options?: Options | undefined | null): Promise<string>
export function hashRaw(password: string | buffer.Buffer, options?: Options | undefined | null): Promise<buffer.Buffer>
export function hashRawSync(password: string | buffer.Buffer, options?: Options | undefined | null): buffer.Buffer
export function hashSync(password: string | buffer.Buffer, options?: Options | undefined | null): string
export function verify(hashed: string | buffer.Buffer, password: string | buffer.Buffer, options?: Options | undefined | null): Promise<boolean>
export function verifySync(hashed: string | buffer.Buffer, password: string | buffer.Buffer, options?: Options | undefined | null): boolean
```

## Usage

```ts
import { hash, verify } from '@ohos-rs/argon2';

const passwordString = 'some_string123';
const passwordBuffer = Buffer.from(passwordString);

const hashed = await hash(passwordBuffer);
await verify(hashed, passwordString)
```