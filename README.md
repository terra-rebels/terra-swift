
<p align="center">
    <a href="https://www.terrarebels.net/"><img src="https://cdn.jim-nielsen.com/macos/512/swift-playgrounds-2020-02-14.png" align="center" width=150/></a>
</p>
<div align="center">
  <sub><em>TerraRebels</em></sub>
  <sub><em>United We Stand</em></sub>
</div>

<p>&nbsp;</p>
<p align="center">
The Official Swift SDK for the Terra Ecosystem (LUNC/USTC/LUNA2)
</p>
<br/>

<p align="center">
  <a href="https://github.com/terra-rebels/terra-sharp/blob/master/LICENSE.md">
  <img alt="GitHub" src="https://img.shields.io/github/license/terra-money/terra.js">
  </a>

  <a href="https://www.nuget.org/packages/TerraSharp">
    <img alt="GitHub" src="https://img.shields.io/nuget/v/TerraSharp">
  </a>
</p>

<p align="center">
  <a href="https://docs.terra.money/"><strong>Explore the Docs »</strong></a>
  <br />
  <br/>
  <a href="https://github.com/terra-rebels/terra-sharp/tree/master/TerraSharp.Maui.Example">Example App</a>
  ·
  <a href="https://terra-rebels.github.io/terra-sharp/TerraSharp/Documentation/html/index.html">API Reference</a>
  ·
  <a href="https://www.nuget.org/packages/TerraSharp">Nuget Package</a>
  ·
  <a href="https://github.com/terra-rebels/Terra-Sharp">GitHub</a>
</p>

TerraSharp is a Swift SDK for writing applications that interact with the Terra blockchain for Mac & iOS environments and provides simple abstractions over core data structures, serialization, key management, and API request generation.

## Features

- **Written in Swift**, with type definitions
- Versatile support for [key management](https://docs.terra.money/docs/develop/sdks/terra-js/keys.html) solutions
- Exposes the Terra API through [`LCDClient`](https://docs.terra.money/docs/develop/sdks/terra-js/query-data.html)
- Parses responses into native Swift types

We highly suggest using TerraSwift in a code editor that has support for type declarations, so you can take advantage of the helpful type hints that are included with the package.

## Installation & Configuration

Grab the latest version off [CocoaPods](https://www.nuget.org/packages/TerraSharp)

```sh
target 'MyApp' do
  pod 'TerraSwift', '~> `1.0'
end
```
Install your dependencies

```sh
pod install
```

Inside your Startup Class (Where you initialize your application), please call the following method, and configure your environment
```cs
// Here we're targeting the Classic Blockchain
TerraStartup.InitializeKernel(TerraEnvironment.Classic);
```
That's it! Now you're ready to start communicating with the blockchain! 

## Usage

Terra-Sharp can be used for Mobile & Web Developers. Supports all Microsoft Technologies from Xamarin, MAUI, ASP & Unity.

### Getting Blockchain data
:exclamation: TerraSharp can connect to both the terra-classic (LUNC/USTC) and LUNA2 networks. If you want to communicate with the classic chain, you have to set your Enviornment on **TerraStartup.InitializeKernel** to **TerraEnvironment.Classic**.

Below we're going to pull balance information on a sample wallet.
```cs

async void FetchBalanceInformation() {
    
    //fetch the LCDClient from the Kernel
    var lcd = TerraStartup.GetService<LCDClient>();
    
    // get the current balance of "terra1x46rqay4d3cssq8gxxvqz8xt6nwlz4td20k38v"
    var balance = await lcd.bank.GetBalance("terra1x46rqay4d3cssq8gxxvqz8xt6nwlz4td20k38v");
    Console.WriteLine(balance);
}

```

### Broadcasting transactions

First, [get](https://faucet.terra.money/) some testnet tokens for "terra1x46rqay4d3cssq8gxxvqz8xt6nwlz4td20k38v", or use [LocalTerra](https://github.com/terra-rebels/LocalTerra).

```cs

async void BroadcastTransaction() {
    
    //fetch the LCDClient from the Kernel
    var lcd = TerraStartup.GetService<LCDClient>();
    
    // create a key out of a mnemonic
    var mk = new MnemonicKey("");
   
    // a wallet can be created out of any key
    // wallets abstract transaction building
    var wallet = lcd.CreateWallet(mk);

    // create a simple message that moves coin balances
    var send = new MsgSend(
      "terra1x46rqay4d3cssq8gxxvqz8xt6nwlz4td20k38v",
      "terra17lmam6zguazs5q5u6z5mmx76uj63gldnse2pdp",
       new { uluna: 1200000}
    );

    await wallet
        .CreateAndSignTx(
          send,
          memo: "test from Terra-Sharp!").ContinueWith(
          (tx) => lcd.tx.Broadcast(tx)).ContinueWith((result) => Console.WriteLine($"TX hash: {result.txhash}");
}
```

## Require Payment Integration for LUNC/USTC?

If you need to integrate with an external payment system or gateway like Apple/Google in app purchases, please make sure to install the [following library](https://github.com/terra-rebels/Terra-Sharp-InAppPurchases) in your project.

## License

This software is licensed under the MIT license. See [LICENSE](./LICENSE) for full disclosure.

© 2022 TerraRebels.
