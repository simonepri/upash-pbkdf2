<h1 align="center">
  <b>phc-pbkdf2</b>
</h1>
<p align="center">
  <!-- CI - TravisCI -->
  <a href="https://travis-ci.org/simonepri/phc-pbkdf2">
    <img src="https://img.shields.io/travis/simonepri/phc-pbkdf2/master.svg?label=MacOS%20%26%20Linux" alt="Mac/Linux Build Status" />
  </a>
  <!-- CI - AppVeyor -->
  <a href="https://ci.appveyor.com/project/simonepri/phc-pbkdf2">
    <img src="https://img.shields.io/appveyor/ci/simonepri/phc-pbkdf2/master.svg?label=Windows" alt="Windows Build status" />
  </a>
  <!-- Coverage - Codecov -->
  <a href="https://codecov.io/gh/simonepri/phc-pbkdf2">
    <img src="https://img.shields.io/codecov/c/github/simonepri/phc-pbkdf2/master.svg" alt="Codecov Coverage report" />
  </a>
  <!-- DM - Snyk -->
  <a href="https://snyk.io/test/github/simonepri/phc-pbkdf2?targetFile=package.json">
    <img src="https://snyk.io/test/github/simonepri/phc-pbkdf2/badge.svg?targetFile=package.json" alt="Known Vulnerabilities" />
  </a>
  <!-- DM - David -->
  <a href="https://david-dm.org/simonepri/phc-pbkdf2">
    <img src="https://david-dm.org/simonepri/phc-pbkdf2/status.svg" alt="Dependency Status" />
  </a>

  <br/>

  <!-- Code Style - XO-Prettier -->
  <a href="https://github.com/xojs/xo">
    <img src="https://img.shields.io/badge/code_style-XO+Prettier-5ed9c7.svg" alt="XO Code Style used" />
  </a>
  <!-- Test Runner - AVA -->
  <a href="https://github.com/avajs/ava">
    <img src="https://img.shields.io/badge/test_runner-AVA-fb3170.svg" alt="AVA Test Runner used" />
  </a>
  <!-- Test Coverage - Istanbul -->
  <a href="https://github.com/istanbuljs/nyc">
    <img src="https://img.shields.io/badge/test_coverage-NYC-fec606.svg" alt="Istanbul Test Coverage used" />
  </a>
  <!-- Init - ni -->
  <a href="https://github.com/simonepri/ni">
    <img src="https://img.shields.io/badge/initialized_with-ni-e74c3c.svg" alt="NI Scaffolding System used" />
  </a>
  <!-- Release - np -->
  <a href="https://github.com/sindresorhus/np">
    <img src="https://img.shields.io/badge/released_with-np-6c8784.svg" alt="NP Release System used" />
  </a>

  <br/>

  <!-- Version - npm -->
  <a href="https://www.npmjs.com/package/@phc/pbkdf2">
    <img src="https://img.shields.io/npm/v/@phc/pbkdf2.svg" alt="Latest version on npm" />
  </a>
  <!-- License - MIT -->
  <a href="https://github.com/simonepri/phc-pbkdf2/tree/master/license">
    <img src="https://img.shields.io/github/license/simonepri/phc-pbkdf2.svg" alt="Project license" />
  </a>
</p>
<p align="center">
  🔒 Node.JS PBKDF2 password hashing algorithm following the PHC string format.
  <br/>

  <sub>
    Coded with ❤️ by <a href="#authors">Simone Primarosa</a>.
  </sub>
</p>

## Synopsis

Protects against brute force, rainbow tables, and timing attacks.

Employs cryptographically secure, per password salts to prevent rainbow table
attacks.  
Key stretching is used to make brute force attacks impractical.  
A constant time verification check prevents variable response time attacks.

## PHC String Format

The [PHC String Format][specs:phc] is an attempt to specify a common hash string format that’s a restricted & well defined subset of the Modular Crypt Format. New hashes are strongly encouraged to adhere to the PHC specification, rather than the much looser [Modular Crypt Format][specs:mcf].

The hash strings generated by this package are in the following format:

```c
$pbkdf2-<digest>$i=<iterations>$<salt>$<hash>
```

Where:

| Field | Type | Description
| --- | --- | --- |
| `<digest>` | <code>string</code> | The [HMAC][specs:HMAC] digest algorithm applied to derive a key of the input password. |
| `<iterations>` | <code>number</code> | The number of iterations desired. The higher the number of iterations, the more secure the derived key will be, but will take a longer amount of time to complete. |
| `<salt>` | <code>string</code> | A sequence of bits, known as a [cryptographic salt][specs:salt] encoded in [B64][specs:B64]. |
| `<hash>` | <code>string</code> | The computed derived key by the [pbkdf2][specs:PBKDF2] algorithm encoded in [B64][specs:B64]. |

## Install

```bash
npm install --save @phc/pbkdf2
```

## Usage

```js
const pbkdf2 = require('@phc/pbkdf2');

// Hash and verify with pbkdf2 and default configs
const hash = await pbkdf2.hash('password');
// => $pbkdf2-sha512$i=10000$O484sW7giRw+nt5WVnp15w$jEUMVZ9adB+63ko/8Dr9oB1jWdndpVVQ65xRlT+tA1GTKcJ7BWlTjdaiILzZAhIPEtgTImKvbgnu8TS/ZrjKgA

const match = await pbkdf2.verify(hash, 'password');
// => true

const match = await pbkdf2.verify(hash, 'wrong');
// => false

const ids = pbkdf2.identifiers();
// => ['pbkdf2-sha1', 'pbkdf2-sha256', 'pbkdf2-sha512']
```

## Benchmarks

Below you can find usage statistics of this hashing algorithm with different
options.  
This should help you understand how the different options affects the running
time and memory usage of the algorithm.

Usage reports are generated thanks to [sympact][gh:sympact].

#### System Report

```
Distro    Release  Platform  Arch
--------  -------  --------  ----
Mac OS X  10.12.6  darwin    x64

CPU     Brand           Clock     Cores
------  --------------  --------  -----
Intel®  Core™ i5-6360U  2.00 GHz  4    

Memory                  Type    Size         Clock   
----------------------  ------  -----------  --------
Micron Technology Inc.  LPDDR3  4294.967 MB  1867 MHz
Micron Technology Inc.  LPDDR3  4294.967 MB  1867 MHz
```

#### 1˙000 iterations

```
CPU Usage (avarage ± σ)  CPU Usage Range (min … max)
-----------------------  ---------------------------
2.65 % ± 1.95 %          0.70 % … 4.60 %            

RAM Usage (avarage ± σ)  RAM Usage Range (min … max)
-----------------------  ---------------------------
23.929 MB ± 0.553 MB     23.376 MB … 24.482 MB      

Execution time  Sampling time  Samples  
--------------  -------------  ---------
0.010 s         0.074 s        2 samples

Instant  CPU Usage  RAM Usage  PIDS
-------  ---------  ---------  -----
0.034 s  0.70 %     23.376 MB  75128
0.074 s  4.60 %     24.482 MB  75128
```

#### 10˙000 iterations

```
CPU Usage (avarage ± σ)  CPU Usage Range (min … max)
-----------------------  ---------------------------
4.35 % ± 3.65 %          0.70 % … 8.00 %            

RAM Usage (avarage ± σ)  RAM Usage Range (min … max)
-----------------------  ---------------------------
23.884 MB ± 0.532 MB     23.351 MB … 24.416 MB      

Execution time  Sampling time  Samples  
--------------  -------------  ---------
0.022 s         0.077 s        2 samples

Instant  CPU Usage  RAM Usage  PIDS
-------  ---------  ---------  -----
0.029 s  0.70 %     23.351 MB  75139
0.077 s  8.00 %     24.416 MB  75139
```

#### 100˙000 iterations

```
CPU Usage (avarage ± σ)  CPU Usage Range (min … max)
-----------------------  ---------------------------
23.09 % ± 16.16 %        0.70 % … 48.00 %           

RAM Usage (avarage ± σ)  RAM Usage Range (min … max)
-----------------------  ---------------------------
24.346 MB ± 0.342 MB     23.380 MB … 24.482 MB      

Execution time  Sampling time  Samples  
--------------  -------------  ---------
0.246 s         0.341 s        9 samples

Instant  CPU Usage  RAM Usage  PIDS
-------  ---------  ---------  -----
0.041 s  0.70 %     23.380 MB  75151
0.118 s  1.80 %     24.461 MB  75151
0.141 s  1.80 %     24.461 MB  75151
0.202 s  31.10 %    24.461 MB  75151
0.233 s  31.10 %    24.461 MB  75151
0.271 s  31.10 %    24.461 MB  75151
0.285 s  31.10 %    24.461 MB  75151
0.297 s  31.10 %    24.482 MB  75151
0.341 s  48.00 %    24.482 MB  75151
```

#### 250˙000 iterations

```
CPU Usage (avarage ± σ)  CPU Usage Range (min … max)
-----------------------  ---------------------------
41.68 % ± 25.04 %        0.90 % … 70.90 %           

RAM Usage (avarage ± σ)  RAM Usage Range (min … max)
-----------------------  ---------------------------
24.301 MB ± 0.258 MB     23.335 MB … 24.388 MB      

Execution time  Sampling time  Samples   
--------------  -------------  ----------
0.405 s         0.457 s        15 samples

Instant  CPU Usage  RAM Usage  PIDS
-------  ---------  ---------  -----
0.028 s  0.90 %     23.335 MB  75176
0.078 s  0.90 %     24.367 MB  75176
0.109 s  0.90 %     24.367 MB  75176
0.150 s  33.70 %    24.367 MB  75176
0.179 s  33.70 %    24.367 MB  75176
0.200 s  33.70 %    24.367 MB  75176
0.226 s  33.70 %    24.367 MB  75176
0.263 s  33.70 %    24.367 MB  75176
0.312 s  56.80 %    24.367 MB  75176
0.330 s  56.80 %    24.367 MB  75176
0.359 s  56.80 %    24.367 MB  75176
0.402 s  70.90 %    24.367 MB  75176
0.430 s  70.90 %    24.367 MB  75176
0.447 s  70.90 %    24.388 MB  75176
0.457 s  70.90 %    24.388 MB  75176
```

## API

#### TOC

<dl>
<dt><a href="#hash">hash(password, [options])</a> ⇒ <code>Promise.&lt;string&gt;</code></dt>
<dd><p>Computes the hash string of the given password in the PHC format using Node&#39;s
built-in crypto.randomBytes() and crypto.pbkdf2().</p>
</dd>
<dt><a href="#verify">verify(password, phcstr)</a> ⇒ <code>Promise.&lt;boolean&gt;</code></dt>
<dd><p>Determines whether or not the stored hash string in PHC format matches the
hash of the password generated for the password provided.</p>
</dd>
<dt><a href="#identifiers">identifiers()</a> ⇒ <code>Array.&lt;string&gt;</code></dt>
<dd><p>Gets the list of all identifiers supported by this hashing function.</p>
</dd>
</dl>

<a name="hash"></a>

### hash(password, [options]) ⇒ <code>Promise.&lt;string&gt;</code>
Computes the hash string of the given password in the PHC format using Node's
built-in crypto.randomBytes() and crypto.pbkdf2().

**Kind**: global function  
**Returns**: <code>Promise.&lt;string&gt;</code> - The generated secure hash string in the PHC
format.  
**Access**: public  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| password | <code>string</code> |  | The password to hash. |
| [options] | <code>Object</code> |  | Optional configurations related to the hashing function. |
| [options.iterations] | <code>number</code> | <code>100000</code> | Optional number of iterations to use. Must be an integer within the range (`0` < `iterations` < `1<<32`). |
| [options.saltSize] | <code>number</code> | <code>16</code> | Optional number of bytes to use when autogenerating new salts. Me be between `0` and `1024`. |
| [options.digest] | <code>string</code> | <code>&quot;sha512&quot;</code> | Optinal name of digest to use when applying the key derivation functions. Can be one of [`'sha1'`, `'sha256'`, `'sha512'`]. |

<a name="verify"></a>

### verify(password, phcstr) ⇒ <code>Promise.&lt;boolean&gt;</code>
Determines whether or not the hash stored inside the PHC formatted string
matches the hash generated for the password provided.

**Kind**: global function  
**Returns**: <code>Promise.&lt;boolean&gt;</code> - A boolean that is true if the hash computed
for the password matches.  
**Access**: public  

| Param | Type | Description |
| --- | --- | --- |
| password | <code>string</code> | User's password input. |
| phcstr | <code>string</code> | Secure hash string generated from this package. |

<a name="identifiers"></a>

### identifiers() ⇒ <code>Array.&lt;string&gt;</code>
Gets the list of all identifiers supported by this hashing function.

**Kind**: global function  
**Returns**: <code>Array.&lt;string&gt;</code> - A list of identifiers supported by this
hashing function.  
**Access**: public

## Contributing

Contributions are REALLY welcome and if you find a security flaw in this code, PLEASE [report it][new issue].  
Please check the [contributing guidelines][contributing] for more details. Thanks!

## Authors

- **Simone Primarosa** - *Github* ([@simonepri][github:simonepri]) • *Twitter* ([@simoneprimarosa][twitter:simoneprimarosa])

See also the list of [contributors][contributors] who participated in this project.

## License

This project is licensed under the MIT License - see the [license][license] file for details.

<!-- Links -->
[start]: https://github.com/simonepri/phc-pbkdf2#start-of-content
[new issue]: https://github.com/simonepri/phc-pbkdf2/issues/new
[contributors]: https://github.com/simonepri/phc-pbkdf2/contributors

[license]: https://github.com/simonepri/phc-pbkdf2/tree/master/license
[contributing]: https://github.com/simonepri/phc-pbkdf2/tree/master/.github/contributing.md

[github:simonepri]: https://github.com/simonepri
[twitter:simoneprimarosa]: http://twitter.com/intent/user?screen_name=simoneprimarosa

[gh:sympact]: https://github.com/simonepri/sympact

[specs:mcf]: https://github.com/ademarre/binary-mcf
[specs:phc]: https://github.com/P-H-C/phc-string-format/blob/master/phc-sf-spec.md
[specs:B64]: https://github.com/P-H-C/phc-string-format/blob/master/phc-sf-spec.md#b64
[specs:salt]: https://en.wikipedia.org/wiki/Salt_(cryptography)
[specs:HMAC]: https://en.wikipedia.org/wiki/HMAC
[specs:PBKDF2]: https://en.wikipedia.org/wiki/PBKDF2
