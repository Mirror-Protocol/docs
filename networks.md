# Interchain Access

Mirror Protocol is an **interchain** DeFi protocol, meaning that it can be accessed and interact with other decentralized applications across multiple blockchains.

Terra blockchain, which Mirror Protocol is built on uses [Shuttle](https://github.com/terra-project/shuttle) bridge to enable interchain transfers between Terra, Ethereum, and Binance Smart Chain.&#x20;

A web application called [Terra Bridge](user-guide/terra-bridge.md) provides web interface to transfer tokens between different blockchains.&#x20;

## Terra

Mirror's core contracts are implemented on the [Terra blockchain](https://terra.money).

### Mainnet (v2)

Network chain ID: `columbus-5`

#### Core Contracts

| Contract          | Address                                                                                                                                    |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Collector         | [terra1s4fllut0e6vw0k3fxsg4fs6fm2ad6hn0prqp3s](https://finder.terra.money/columbus-4/account/terra1s4fllut0e6vw0k3fxsg4fs6fm2ad6hn0prqp3s) |
| Community         | [terra1x35fvy3sy47drd3qs288sm47fjzjnksuwpyl9k](https://finder.terra.money/columbus-4/account/terra1x35fvy3sy47drd3qs288sm47fjzjnksuwpyl9k) |
| Factory           | [terra1mzj9nsxx0lxlaxnekleqdy8xnyw2qrh3uz6h8p](https://finder.terra.money/columbus-4/account/terra1mzj9nsxx0lxlaxnekleqdy8xnyw2qrh3uz6h8p) |
| Gov               | [terra1wh39swv7nq36pnefnupttm2nr96kz7jjddyt2x](https://finder.terra.money/columbus-4/account/terra1wh39swv7nq36pnefnupttm2nr96kz7jjddyt2x) |
| Admin Manager     | [terra138tljkg697vwlvatzzfc7r5lfyhkxt7n7cudce](https://finder.terra.money/mainnet/address/terra138tljkg697vwlvatzzfc7r5lfyhkxt7n7cudce)    |
| Mint              | [terra1wfz7h3aqf4cjmjcvc6s8lxdhh7k30nkczyf0mj](https://finder.terra.money/columbus-4/account/terra1wfz7h3aqf4cjmjcvc6s8lxdhh7k30nkczyf0mj) |
| Oracle Hub        | [terra1t5k2c2p2kf5as247egz53rj8g8g2x4jw9qte9a](https://finder.terra.money/mainnet/address/terra1t5k2c2p2kf5as247egz53rj8g8g2x4jw9qte9a)    |
| Staking           | [terra17f7zu97865jmknk7p2glqvxzhduk78772ezac5](https://finder.terra.money/columbus-4/account/terra17f7zu97865jmknk7p2glqvxzhduk78772ezac5) |
| Airdrop           | [terra1kalp2knjm4cs3f59ukr4hdhuuncp648eqrgshw](https://finder.terra.money/columbus-4/account/terra1kalp2knjm4cs3f59ukr4hdhuuncp648eqrgshw) |
| Limit Order       | [terra1zpr8tq3ts96mthcdkukmqq4y9lhw0ycevsnw89](https://finder.terra.money/columbus-4/address/terra1zpr8tq3ts96mthcdkukmqq4y9lhw0ycevsnw89) |
| Collateral Oracle | [terra1pmlh0j5gpzh2wsmyd3cuk39cgh2gfwk6h5wy9j](https://finder.terra.money/columbus-4/address/terra1pmlh0j5gpzh2wsmyd3cuk39cgh2gfwk6h5wy9j) |
| Lock              | [terra169urmlm8wcltyjsrn7gedheh7dker69ujmerv2](https://finder.terra.money/columbus-4/address/terra169urmlm8wcltyjsrn7gedheh7dker69ujmerv2) |
| Short Reward      | [terra16mlzdwqq5qs6a23250lq0fftke8v80sapc5kye](https://finder.terra.money/columbus-4/address/terra16mlzdwqq5qs6a23250lq0fftke8v80sapc5kye) |

#### Asset Contracts

| Asset  | Mainnet                                                                                                                                    |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| MIR    | [terra15gwkyepfc6xgca5t5zefzwy42uts8l2m4g40k6](https://finder.terra.money/columbus-4/account/terra15gwkyepfc6xgca5t5zefzwy42uts8l2m4g40k6) |
| mAAPL  | [terra1vxtwu4ehgzz77mnfwrntyrmgl64qjs75mpwqaz](https://finder.terra.money/columbus-4/account/terra1vxtwu4ehgzz77mnfwrntyrmgl64qjs75mpwqaz) |
| mGOOGL | [terra1h8arz2k547uvmpxctuwush3jzc8fun4s96qgwt](https://finder.terra.money/columbus-4/account/terra1h8arz2k547uvmpxctuwush3jzc8fun4s96qgwt) |
| mTSLA  | [terra14y5affaarufk3uscy2vr6pe6w6zqf2wpjzn5sh](https://finder.terra.money/columbus-4/account/terra14y5affaarufk3uscy2vr6pe6w6zqf2wpjzn5sh) |
| mNFLX  | [terra1jsxngqasf2zynj5kyh0tgq9mj3zksa5gk35j4k](https://finder.terra.money/columbus-4/account/terra1jsxngqasf2zynj5kyh0tgq9mj3zksa5gk35j4k) |
| mQQQ   | [terra1csk6tc7pdmpr782w527hwhez6gfv632tyf72cp](https://finder.terra.money/columbus-4/account/terra1csk6tc7pdmpr782w527hwhez6gfv632tyf72cp) |
| mTWTR  | [terra1cc3enj9qgchlrj34cnzhwuclc4vl2z3jl7tkqg](https://finder.terra.money/columbus-4/account/terra1cc3enj9qgchlrj34cnzhwuclc4vl2z3jl7tkqg) |
| mMSFT  | [terra1227ppwxxj3jxz8cfgq00jgnxqcny7ryenvkwj6](https://finder.terra.money/columbus-4/account/terra1227ppwxxj3jxz8cfgq00jgnxqcny7ryenvkwj6) |
| mAMZN  | [terra165nd2qmrtszehcfrntlplzern7zl4ahtlhd5t2](https://finder.terra.money/columbus-4/account/terra165nd2qmrtszehcfrntlplzern7zl4ahtlhd5t2) |
| mBABA  | [terra1w7zgkcyt7y4zpct9dw8mw362ywvdlydnum2awa](https://finder.terra.money/columbus-4/account/terra1w7zgkcyt7y4zpct9dw8mw362ywvdlydnum2awa) |
| mIAU   | [terra15hp9pr8y4qsvqvxf3m4xeptlk7l8h60634gqec](https://finder.terra.money/columbus-4/account/terra15hp9pr8y4qsvqvxf3m4xeptlk7l8h60634gqec) |
| mSLV   | [terra1kscs6uhrqwy6rx5kuw5lwpuqvm3t6j2d6uf2lp](https://finder.terra.money/columbus-4/account/terra1kscs6uhrqwy6rx5kuw5lwpuqvm3t6j2d6uf2lp) |
| mUSO   | [terra1lvmx8fsagy70tv0fhmfzdw9h6s3sy4prz38ugf](https://finder.terra.money/columbus-4/account/terra1lvmx8fsagy70tv0fhmfzdw9h6s3sy4prz38ugf) |
| mVIXY  | [terra1zp3a6q6q4953cz376906g5qfmxnlg77hx3te45](https://finder.terra.money/columbus-4/account/terra1zp3a6q6q4953cz376906g5qfmxnlg77hx3te45) |
| mFB    | [terra1mqsjugsugfprn3cvgxsrr8akkvdxv2pzc74us7](https://finder.terra.money/columbus-4/account/terra1mqsjugsugfprn3cvgxsrr8akkvdxv2pzc74us7) |
| mCOIN  | [terra18wayjpyq28gd970qzgjfmsjj7dmgdk039duhph](https://finder.terra.money/columbus-4/address/terra18wayjpyq28gd970qzgjfmsjj7dmgdk039duhph) |
| mHOOD  | [terra18yqdfzfhnguerz9du5mnvxsh5kxlknqhcxzjfr](https://finder.terra.money/columbus-5/address/terra18yqdfzfhnguerz9du5mnvxsh5kxlknqhcxzjfr) |
| mARKK  | [terra1qqfx5jph0rsmkur2zgzyqnfucra45rtjae5vh6](https://finder.terra.money/columbus-5/address/terra1qqfx5jph0rsmkur2zgzyqnfucra45rtjae5vh6) |
| mGLXY  | [terra1l5lrxtwd98ylfy09fn866au6dp76gu8ywnudls](https://finder.terra.money/columbus-5/address/terra1l5lrxtwd98ylfy09fn866au6dp76gu8ywnudls) |
| mSQ    | [terra1u43zu5amjlsgty5j64445fr9yglhm53m576ugh](https://finder.terra.money/columbus-5/address/terra1u43zu5amjlsgty5j64445fr9yglhm53m576ugh) |
| mABNB  | [terra1g4x2pzmkc9z3mseewxf758rllg08z3797xly0n](https://finder.terra.money/columbus-5/address/terra1g4x2pzmkc9z3mseewxf758rllg08z3797xly0n) |
| mSPY   | [terra1aa00lpfexyycedfg5k2p60l9djcmw0ue5l8fhc](https://finder.terra.money/columbus-5/address/terra1aa00lpfexyycedfg5k2p60l9djcmw0ue5l8fhc) |
| mDOT   | [terra19ya4jpvjvvtggepvmmj6ftmwly3p7way0tt08r](https://finder.terra.money/columbus-5/address/terra19ya4jpvjvvtggepvmmj6ftmwly3p7way0tt08r) |
| mAMD   | [terra18ej5nsuu867fkx4tuy2aglpvqjrkcrjjslap3z](https://finder.terra.money/columbus-5/address/terra18ej5nsuu867fkx4tuy2aglpvqjrkcrjjslap3z) |
| mGME   | [terra1m6j6j9gw728n82k78s0j9kq8l5p6ne0xcc820p](https://finder.terra.money/columbus-5/address/terra1m6j6j9gw728n82k78s0j9kq8l5p6ne0xcc820p) |
| mAMC   | [terra1qelfthdanju7wavc5tq0k5r0rhsyzyyrsn09qy](https://finder.terra.money/columbus-5/address/terra1qelfthdanju7wavc5tq0k5r0rhsyzyyrsn09qy) |
| mGS    | [terra137drsu8gce5thf6jr5mxlfghw36rpljt3zj73v](networks.md#terra)                                                                          |
| mKO    | [terra1qsnj5gvq8rgs7yws8x5u02gwd5wvtu4tks0hjm](https://finder.terra.money/mainnet/address/terra1qsnj5gvq8rgs7yws8x5u02gwd5wvtu4tks0hjm)    |
| mPYPL  | [terra1rh2907984nudl7vh56qjdtvv7947z4dujj92sx](https://finder.terra.money/mainnet/address/terra1rh2907984nudl7vh56qjdtvv7947z4dujj92sx)    |
| mSBUX  | [terra1246zy658dfgtausf0c4a6ly8sc2e285q4kxqga](https://finder.terra.money/mainnet/address/terra1246zy658dfgtausf0c4a6ly8sc2e285q4kxqga)    |
| mJNJ   | [terra1ptdxmj3xmmljzx02nr4auwfuelmj0cnkh8egs2](https://finder.terra.money/mainnet/address/terra1ptdxmj3xmmljzx02nr4auwfuelmj0cnkh8egs2)    |
| mNVDA  | [terra1drsjzvzej4h4qlehcfwclxg4w5l3h5tuvd3jd8](https://finder.terra.money/mainnet/address/terra1drsjzvzej4h4qlehcfwclxg4w5l3h5tuvd3jd8)    |
| mDIS   | [terra149755r3y0rve30e209awkhn5cxgkn5c8ju9pm5](https://finder.terra.money/mainnet/address/terra149755r3y0rve30e209awkhn5cxgkn5c8ju9pm5)    |

### Testnet (v2)

Network chain ID: `bombay-12`

#### Core Contracts

| Contract          | Address                                                                                                                                      |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Collector         | [terra1v046ktavwzlyct5gh8ls767fh7hc4gxc95grxy](https://finder.terra.money/tequila-0004/account/terra1v046ktavwzlyct5gh8ls767fh7hc4gxc95grxy) |
| Community         | [terra10qm80sfht0zhh3gaeej7sd4f92tswc44fn000q](https://finder.terra.money/tequila-0004/account/terra10qm80sfht0zhh3gaeej7sd4f92tswc44fn000q) |
| Factory           | [terra10l9xc9eyrpxd5tqjgy6uxrw7dd9cv897cw8wdr](https://finder.terra.money/tequila-0004/account/terra10l9xc9eyrpxd5tqjgy6uxrw7dd9cv897cw8wdr) |
| Gov               | [terra12r5ghc6ppewcdcs3hkewrz24ey6xl7mmpk478s](https://finder.terra.money/tequila-0004/account/terra12r5ghc6ppewcdcs3hkewrz24ey6xl7mmpk478s) |
| Admin Manager     | [terra198rvl2eq3pum8skrr97vj0pqtugt3xns0lmdkc](https://finder.terra.money/testnet/address/terra198rvl2eq3pum8skrr97vj0pqtugt3xns0lmdkc)      |
| Mint              | [terra1s9ehcjv0dqj2gsl72xrpp0ga5fql7fj7y3kq3w](https://finder.terra.money/tequila-0004/account/terra1s9ehcjv0dqj2gsl72xrpp0ga5fql7fj7y3kq3w) |
| Oracle Hub        | [terra1sdr3rya4h039f4htfm42q44x3dlaxra7hc7p8e](https://finder.terra.money/testnet/address/terra1sdr3rya4h039f4htfm42q44x3dlaxra7hc7p8e)      |
| Staking           | [terra1a06dgl27rhujjphsn4drl242ufws267qxypptx](https://finder.terra.money/tequila-0004/account/terra1a06dgl27rhujjphsn4drl242ufws267qxypptx) |
| Airdrop           | [terra1p6nvyw7vz3fgpy4nyh3q3vc09e65sr97ejxn2p](https://finder.terra.money/tequila-0004/account/terra1p6nvyw7vz3fgpy4nyh3q3vc09e65sr97ejxn2p) |
| Limit Order       | [terra1vc4ch0z3n6c23f9uywzy5yqaj2gmpnam8qgge7](https://finder.terra.money/tequila-0004/account/terra1vc4ch0z3n6c23f9uywzy5yqaj2gmpnam8qgge7) |
| Collateral Oracle | [terra1q3ls6u2glsazdeu7dxggk8d04elnvmsg0ung6n](https://finder.terra.money/tequila-0004/account/terra1q3ls6u2glsazdeu7dxggk8d04elnvmsg0ung6n) |
| Lock              | [terra1pcxghd4dyf950mcs0kmlp7lvnrjsnl6qlfldwj](https://finder.terra.money/tequila-0004/address/terra1pcxghd4dyf950mcs0kmlp7lvnrjsnl6qlfldwj) |

#### Asset Contracts

| Asset  | Address                                                                                                                                      |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| MIR    | [terra10llyp6v3j3her8u3ce66ragytu45kcmd9asj3u](https://finder.terra.money/tequila-0004/account/terra10llyp6v3j3her8u3ce66ragytu45kcmd9asj3u) |
| mAAPL  | [terra16vfxm98rxlc8erj4g0sj5932dvylgmdufnugk0](https://finder.terra.money/tequila-0004/account/terra16vfxm98rxlc8erj4g0sj5932dvylgmdufnugk0) |
| mGOOGL | [terra1qg9ugndl25567u03jrr79xur2yk9d632fke3h2](https://finder.terra.money/tequila-0004/account/terra1qg9ugndl25567u03jrr79xur2yk9d632fke3h2) |
| mTSLA  | [terra1nslem9lgwx53rvgqwd8hgq7pepsry6yr3wsen4](https://finder.terra.money/tequila-0004/account/terra1nslem9lgwx53rvgqwd8hgq7pepsry6yr3wsen4) |
| mNFLX  | [terra1djnlav60utj06kk9dl7defsv8xql5qpryzvm3h](https://finder.terra.money/tequila-0004/account/terra1djnlav60utj06kk9dl7defsv8xql5qpryzvm3h) |
| mQQQ   | [terra18yx7ff8knc98p07pdkhm3u36wufaeacv47fuha](https://finder.terra.money/tequila-0004/account/terra18yx7ff8knc98p07pdkhm3u36wufaeacv47fuha) |
| mTWTR  | [terra1ax7mhqahj6vcqnnl675nqq2g9wghzuecy923vy](https://finder.terra.money/tequila-0004/account/terra1ax7mhqahj6vcqnnl675nqq2g9wghzuecy923vy) |
| mMSFT  | [terra12s2h8vlztjwu440khpc0063p34vm7nhu25w4p9](https://finder.terra.money/tequila-0004/account/terra12s2h8vlztjwu440khpc0063p34vm7nhu25w4p9) |
| mAMZN  | [terra12saaecsqwxj04fn0jsv4jmdyp6gylptf5tksge](https://finder.terra.money/tequila-0004/account/terra12saaecsqwxj04fn0jsv4jmdyp6gylptf5tksge) |
| mBABA  | [terra15dr4ah3kha68kam7a907pje9w6z2lpjpnrkd06](https://finder.terra.money/tequila-0004/account/terra15dr4ah3kha68kam7a907pje9w6z2lpjpnrkd06) |
| mIAU   | [terra19dl29dpykvzej8rg86mjqg8h63s9cqvkknpclr](https://finder.terra.money/tequila-0004/account/terra19dl29dpykvzej8rg86mjqg8h63s9cqvkknpclr) |
| mSLV   | [terra1fdkfhgk433tar72t4edh6p6y9rmjulzc83ljuw](https://finder.terra.money/tequila-0004/account/terra1fdkfhgk433tar72t4edh6p6y9rmjulzc83ljuw) |
| mUSO   | [terra1fucmfp8x4mpzsydjaxyv26hrkdg4vpdzdvf647](https://finder.terra.money/tequila-0004/account/terra1fucmfp8x4mpzsydjaxyv26hrkdg4vpdzdvf647) |
| mVIXY  | [terra1z0k7nx0vl85hwpv3e3hu2cyfkwq07fl7nqchvd](https://finder.terra.money/tequila-0004/account/terra1z0k7nx0vl85hwpv3e3hu2cyfkwq07fl7nqchvd) |
| mFB    | [terra14gq9wj0tt6vu0m4ec2tkkv4ln3qrtl58lgdl2c](https://finder.terra.money/tequila-0004/account/terra14gq9wj0tt6vu0m4ec2tkkv4ln3qrtl58lgdl2c) |
| mCOIN  | [terra1qre9crlfnulcg0m68qqywqqstplgvrzywsg3am](https://finder.terra.money/tequila-0004/address/terra1qre9crlfnulcg0m68qqywqqstplgvrzywsg3am) |
| mHOOD  | [terra179na3xcvjastpptnh9g6lnf75hqqjnsv9mqm3j](https://finder.terra.money/bombay-12/address/terra1aa00lpfexyycedfg5k2p60l9djcmw0ue5l8fhc)    |
| mARKK  | [terra1qk0cd8576fqf33paf40xy3rt82p7yluwtxz7qx](https://finder.terra.money/bombay-12/address/terra1qk0cd8576fqf33paf40xy3rt82p7yluwtxz7qx)    |
| mSQ    | [terra18qs6704f4ujnwus9x9vxcxrrm0du0f232kpnd6](https://finder.terra.money/bombay-12/address/terra18qs6704f4ujnwus9x9vxcxrrm0du0f232kpnd6)    |
| mABNB  | [terra1avryzxnsn2denq7p2d7ukm6nkck9s0rz2llgnc](https://finder.terra.money/bombay-12/address/terra1avryzxnsn2denq7p2d7ukm6nkck9s0rz2llgnc)    |
| mSPY   | [terra15t9afkpj0wnh8m74n8n2f8tspkn7r65vnru45s](https://finder.terra.money/bombay-12/address/terra15t9afkpj0wnh8m74n8n2f8tspkn7r65vnru45s)    |
| mDOT   | [terra1fs6c6y65c65kkjanjwvmnrfvnm2s58ph88t9ky](https://finder.terra.money/bombay-12/address/terra1fs6c6y65c65kkjanjwvmnrfvnm2s58ph88t9ky)    |
| mAMD   | [terra1ftefjmtpnk2fctsa8xkv8naven65z5qtgq83nu](https://finder.terra.money/bombay-12/address/terra1ftefjmtpnk2fctsa8xkv8naven65z5qtgq83nu)    |
| mGME   | [terra104tgj4gc3pp5s240a0mzqkhd3jzkn8v0u07hlf](https://finder.terra.money/bombay-12/address/terra104tgj4gc3pp5s240a0mzqkhd3jzkn8v0u07hlf)    |
| mAMC   | [terra1zeyfhurlrun6sgytqfue555e6vw2ndxt2j7jhd](https://finder.terra.money/bombay-12/address/terra1zeyfhurlrun6sgytqfue555e6vw2ndxt2j7jhd)    |
| mGS    | [terra13myzfjdmvqkama2tt3v5f7quh75rv78w8kq6u6](https://finder.terra.money/bombay-12/address/terra13myzfjdmvqkama2tt3v5f7quh75rv78w8kq6u6)    |
| mBTC   | [terra1csr22xvxs6r3gkjsl7pmjkmpt39mwjsrm0e2r8](https://finder.terra.money/testnet/address/terra1csr22xvxs6r3gkjsl7pmjkmpt39mwjsrm0e2r8)      |
| mETH   | [terra1ys4dwwzaenjg2gy02mslmc96f267xvpsjat7gx](https://finder.terra.money/testnet/address/terra1ys4dwwzaenjg2gy02mslmc96f267xvpsjat7gx)      |
|        |                                                                                                                                              |

## Ethereum

Mirror on Ethereum is available via a bridge called [Shuttle](https://github.com/terra-project/shuttle), which facilitates cross-chain transfers of assets between Terra and Ethereum networks (mainnets and testnets supported).

### Mainnet

Ethereum Mirror contracts are available at the following addresses:

| Asset  | Token Address                                                                                                         |
| ------ | --------------------------------------------------------------------------------------------------------------------- |
| LUNA   | [0xd2877702675e6cEb975b4A1dFf9fb7BAF4C91ea9](https://etherscan.io/token/0xd2877702675e6cEb975b4A1dFf9fb7BAF4C91ea9)   |
| UST    | [0xa47c8bf37f92aBed4A126BDA807A7b7498661acD](https://etherscan.io/token/0xa47c8bf37f92aBed4A126BDA807A7b7498661acD)   |
| KRT    | [0xcAAfF72A8CbBfc5Cf343BA4e26f65a257065bFF1](https://etherscan.io/token/0xcAAfF72A8CbBfc5Cf343BA4e26f65a257065bFF1)   |
| SDT    | [0x676Ad1b33ae6423c6618C1AEcf53BAa29cf39EE5](https://etherscan.io/token/0x676Ad1b33ae6423c6618C1AEcf53BAa29cf39EE5)   |
| MNT    | [0x156B36ec68FdBF84a925230BA96cb1Ca4c4bdE45](https://etherscan.io/token/0x156B36ec68FdBF84a925230BA96cb1Ca4c4bdE45)   |
| MIR    | [0x09a3EcAFa817268f77BE1283176B946C4ff2E608](https://etherscan.io/token/0x09a3EcAFa817268f77BE1283176B946C4ff2E608)   |
| mAAPL  | [0xd36932143F6eBDEDD872D5Fb0651f4B72Fd15a84](https://etherscan.io/token/0xd36932143F6eBDEDD872D5Fb0651f4B72Fd15a84)   |
| mGOOGL | [0x59A921Db27Dd6d4d974745B7FfC5c33932653442](https://etherscan.io/token/0x59A921Db27Dd6d4d974745B7FfC5c33932653442)   |
| mTSLA  | [0x21cA39943E91d704678F5D00b6616650F066fD63](https://etherscan.io/token/0x21cA39943E91d704678F5D00b6616650F066fD63)   |
| mNFLX  | [0xC8d674114bac90148d11D3C1d33C61835a0F9DCD](https://etherscan.io/token/0xC8d674114bac90148d11D3C1d33C61835a0F9DCD)   |
| mQQQ   | [0x13B02c8dE71680e71F0820c996E4bE43c2F57d15](https://etherscan.io/token/0x13B02c8dE71680e71F0820c996E4bE43c2F57d15)   |
| mTWTR  | [0xEdb0414627E6f1e3F082DE65cD4F9C693D78CCA9](https://etherscan.io/token/0xEdb0414627E6f1e3F082DE65cD4F9C693D78CCA9)   |
| mMSFT  | [0x41BbEDd7286dAab5910a1f15d12CBda839852BD7](https://etherscan.io/token/0x41BbEDd7286dAab5910a1f15d12CBda839852BD7)   |
| mAMZN  | [0x0cae9e4d663793c2a2A0b211c1Cf4bBca2B9cAa7](https://etherscan.io/token/0x0cae9e4d663793c2a2A0b211c1Cf4bBca2B9cAa7)   |
| mBABA  | [0x56aA298a19C93c6801FDde870fA63EF75Cc0aF72](https://etherscan.io/token/0x56aA298a19C93c6801FDde870fA63EF75Cc0aF72)   |
| mIAU   | [0x1d350417d9787E000cc1b95d70E9536DcD91F373](https://etherscan.io/token/0x1d350417d9787E000cc1b95d70E9536DcD91F373)   |
| mSLV   | [0x9d1555d8cB3C846Bb4f7D5B1B1080872c3166676](https://etherscan.io/token/0x9d1555d8cB3C846Bb4f7D5B1B1080872c3166676)   |
| mUSO   | [0x31c63146a635EB7465e5853020b39713AC356991](https://etherscan.io/token/0x31c63146a635EB7465e5853020b39713AC356991)   |
| mVIXY  | [0xf72FCd9DCF0190923Fadd44811E240Ef4533fc86](https://etherscan.io/token/0xf72FCd9DCF0190923Fadd44811E240Ef4533fc86)   |
| mFB    | [0x0e99cC0535BB6251F6679Fa6E65d6d3b430e840B](https://etherscan.io/token/0x0e99cC0535BB6251F6679Fa6E65d6d3b430e840B)   |
| mCOIN  | [0x1e25857931F75022a8814e0B0c3a371942A88437](https://etherscan.io/address/0x1e25857931F75022a8814e0B0c3a371942A88437) |

### Testnet

Ethereum Mirror contracts are available on the Ropsten (PoW) testnet.

| Asset  | Address                                                                                                                       |
| ------ | ----------------------------------------------------------------------------------------------------------------------------- |
| LUNA   | [0xbf51453468771D14cEbdF8856cC5D5145364Cd6F](https://ropsten.etherscan.io/token/0xbf51453468771D14cEbdF8856cC5D5145364Cd6F)   |
| UST    | [0x6cA13a4ab78dd7D657226b155873A04DB929A3A4](https://ropsten.etherscan.io/token/0x6cA13a4ab78dd7D657226b155873A04DB929A3A4)   |
| KRT    | [0xF0b0fB87017b644eC76644Ea0FA704BFA5f20F0E](https://ropsten.etherscan.io/token/0xF0b0fB87017b644eC76644Ea0FA704BFA5f20F0E)   |
| SDT    | [0x1d805d8660Ae73E3624AECAa34ca5FcF8E26E0a5](https://ropsten.etherscan.io/token/0x1d805d8660Ae73E3624AECAa34ca5FcF8E26E0a5)   |
| MNT    | [0x51e7f3ED326719a1469EbD7E68B8AB963d64eBA6](https://ropsten.etherscan.io/token/0x51e7f3ED326719a1469EbD7E68B8AB963d64eBA6)   |
| MIR    | [0xDAdC10D2dAC9E111835d4423670573Ae45714e7C](https://ropsten.etherscan.io/token/0xDAdC10D2dAC9E111835d4423670573Ae45714e7C)   |
| mAAPL  | [0xDAE57D13b42325562963C1E47E615eE25924635C](https://ropsten.etherscan.io/token/0xDAE57D13b42325562963C1E47E615eE25924635C)   |
| mGOOGL | [0x58E3ba48E036341EF8Bbe0bF49caA9731Cc5C42B](https://ropsten.etherscan.io/token/0x58E3ba48E036341EF8Bbe0bF49caA9731Cc5C42B)   |
| mTSLA  | [0x2a445f4dA6Ea8845c594446b250ad535373bb7e4](https://ropsten.etherscan.io/token/0x2a445f4dA6Ea8845c594446b250ad535373bb7e4)   |
| mNFLX  | [0x1EA12ca0Ac017EfFE87ddF4c648a1a5359E850FA](https://ropsten.etherscan.io/token/0x1EA12ca0Ac017EfFE87ddF4c648a1a5359E850FA)   |
| mQQQ   | [0xE1d4509C539D9C3f1E01CeE22e7a79BF77348Ef3](https://ropsten.etherscan.io/token/0xE1d4509C539D9C3f1E01CeE22e7a79BF77348Ef3)   |
| mTWTR  | [0x0c9149d38AD1eBE71c50Bd04E0Ba4F999884C961](https://ropsten.etherscan.io/token/0x0c9149d38AD1eBE71c50Bd04E0Ba4F999884C961)   |
| mMSFT  | [0x0736644C0257048861bAa72b6b234514c6b52655](https://ropsten.etherscan.io/token/0x0736644C0257048861bAa72b6b234514c6b52655)   |
| mAMZN  | [0x3210BC26eB5427D0FC19dE7AB272b3BB3e4bC4b0](https://ropsten.etherscan.io/token/0x3210BC26eB5427D0FC19dE7AB272b3BB3e4bC4b0)   |
| mBABA  | [0xF44c4C095E586B5a7Ba8AA0B2A8Dfad693d396b6](https://ropsten.etherscan.io/token/0xF44c4C095E586B5a7Ba8AA0B2A8Dfad693d396b6)   |
| mIAU   | [0x51eD1489e3D311496592056608dD6cf025C03525](https://ropsten.etherscan.io/token/0x51eD1489e3D311496592056608dD6cf025C03525)   |
| mSLV   | [0xECBe84E79bb26a7FF2474AA1b58d2696A9b5F58F](https://ropsten.etherscan.io/token/0xECBe84E79bb26a7FF2474AA1b58d2696A9b5F58F)   |
| mUSO   | [0xDF00833C87bEfA3aF5634d81BE18E9DEf2F9C7c0](https://ropsten.etherscan.io/token/0xDF00833C87bEfA3aF5634d81BE18E9DEf2F9C7c0)   |
| mVIXY  | [0xC1629641Cdb2D636Ae220fb759264306902c4AC0](https://ropsten.etherscan.io/token/0xC1629641Cdb2D636Ae220fb759264306902c4AC0)   |
| mFB    | [0x0Add4875eBcbD2306921e12133feB562E1cc82b4](https://ropsten.etherscan.io/address/0x0Add4875eBcbD2306921e12133feB562E1cc82b4) |
| mCOIN  | [0x807eD0f8149E66Cb74E340bbB298a28E9233181c](https://ropsten.etherscan.io/address/0x807eD0f8149E66Cb74E340bbB298a28E9233181c) |

## Binance Smart Chain (BSC)

Mirrored Assets are accessible on Binance Smart Chain via a bridge called [Shuttle](https://github.com/terra-project/shuttle), which facilitates cross-chain transfers of assets between Terra and BSC networks (mainnets and testnets supported).

### Mainnet

Binance Smart Chain Mirror contracts are available at the following addresses:

| Asset  | Address                                                                                                              |
| ------ | -------------------------------------------------------------------------------------------------------------------- |
| LUNA   | [0xECCF35F941Ab67FfcAA9A1265C2fF88865caA005](https://bscscan.com/address/0xECCF35F941Ab67FfcAA9A1265C2fF88865caA005) |
| UST    | [0x23396cF899Ca06c4472205fC903bDB4de249D6fC](https://bscscan.com/address/0x23396cf899ca06c4472205fc903bdb4de249d6fc) |
| KRT    | [0xfFBDB9BDCae97a962535479BB96cC2778D65F4dd](https://bscscan.com/address/0xffbdb9bdcae97a962535479bb96cc2778d65f4dd) |
| SDT    | [0x7d5f9F8CF59986743f34BC137Fc197E2e22b7B05](https://bscscan.com/address/0x7d5f9F8CF59986743f34BC137Fc197E2e22b7B05) |
| MNT    | [0x41D74991509318517226755E508695c4D1CE43a6](https://bscscan.com/address/0x41D74991509318517226755E508695c4D1CE43a6) |
| MIR    | [0x5B6DcF557E2aBE2323c48445E8CC948910d8c2c9](https://bscscan.com/address/0x5B6DcF557E2aBE2323c48445E8CC948910d8c2c9) |
| mAAPL  | [0x900AEb8c40b26A8f8DfAF283F884b03EE7Abb3Ec](https://bscscan.com/address/0x900AEb8c40b26A8f8DfAF283F884b03EE7Abb3Ec) |
| mGOOGL | [0x62D71B23bF15218C7d2D7E48DBbD9e9c650B173f](https://bscscan.com/address/0x62D71B23bF15218C7d2D7E48DBbD9e9c650B173f) |
| mTSLA  | [0xF215A127A196e3988C09d052e16BcFD365Cd7AA3](https://bscscan.com/address/0xF215A127A196e3988C09d052e16BcFD365Cd7AA3) |
| mNFLX  | [0xa04F060077D90Fe2647B61e4dA4aD1F97d6649dc](https://bscscan.com/address/0xa04F060077D90Fe2647B61e4dA4aD1F97d6649dc) |
| mQQQ   | [0x1Cb4183Ac708e07511Ac57a2E45A835F048D7C56](https://bscscan.com/address/0x1Cb4183Ac708e07511Ac57a2E45A835F048D7C56) |
| mTWTR  | [0x7426Ab52A0e057691E2544fae9C8222e958b2cfB](https://bscscan.com/address/0x7426Ab52A0e057691E2544fae9C8222e958b2cfB) |
| mMSFT  | [0x0ab06caa3Ca5d6299925EfaA752A2D2154ECE929](https://bscscan.com/address/0x0ab06caa3Ca5d6299925EfaA752A2D2154ECE929) |
| mAMZN  | [0x3947B992DC0147D2D89dF0392213781b04B25075](https://bscscan.com/address/0x3947B992DC0147D2D89dF0392213781b04B25075) |
| mBABA  | [0xcA2f75930912B85d8B2914Ad06166483c0992945](https://bscscan.com/address/0xcA2f75930912B85d8B2914Ad06166483c0992945) |
| mIAU   | [0x1658AeD6C7dbaB2Ddbd8f5D898b0e9eAb0305813](https://bscscan.com/address/0x1658AeD6C7dbaB2Ddbd8f5D898b0e9eAb0305813) |
| mSLV   | [0x211e763d0b9311c08EC92D72DdC20AB024b6572A](https://bscscan.com/address/0x211e763d0b9311c08EC92D72DdC20AB024b6572A) |
| mUSO   | [0x9cDDF33466cE007676C827C76E799F5109f1843C](https://bscscan.com/address/0x9cDDF33466cE007676C827C76E799F5109f1843C) |
| mVIXY  | [0x92E744307694Ece235cd02E82680ec37c657D23E](https://bscscan.com/address/0x92E744307694Ece235cd02E82680ec37c657D23E) |
| mFB    | [0x5501F4713020cf299C3C5929da549Aab3592E451](https://bscscan.com/address/0x5501F4713020cf299C3C5929da549Aab3592E451) |
| mCOIN  | [0x49022089e78a8D46Ec87A3AF86a1Db6c189aFA6f](https://bscscan.com/address/0x49022089e78a8D46Ec87A3AF86a1Db6c189aFA6f) |

### Testnet

Binance Smart Chain Mirror contracts are available on BSC Testnet.



| Asset  | Address                                                                                                                      |
| ------ | ---------------------------------------------------------------------------------------------------------------------------- |
| LUNA   | [0xA1B4Aa780713df91e9Fa0FAa415ce49756D81E3b](https://testnet.bscscan.com/address/0xA1B4Aa780713df91e9Fa0FAa415ce49756D81E3b) |
| UST    | [0x66BDf3Bd407A63eAB5eAF5eCE69f2D7bb403EfC9](https://testnet.bscscan.com/address/0x66BDf3Bd407A63eAB5eAF5eCE69f2D7bb403EfC9) |
| KRT    | [0x59a870b16adE2A152815Ba0d4Fa074fc3F71A828](https://testnet.bscscan.com/address/0x59a870b16adE2A152815Ba0d4Fa074fc3F71A828) |
| SDT    | [0x5e2c2088d3fB10aAb25a0D323CdBEc5147232B1a](https://testnet.bscscan.com/address/0x5e2c2088d3fB10aAb25a0D323CdBEc5147232B1a) |
| MNT    | [0x1449D1Ba8FB922E74F7761F077e77EAe66A0f8DA](https://testnet.bscscan.com/address/0x1449D1Ba8FB922E74F7761F077e77EAe66A0f8DA) |
| MIR    | [0x320106A19C934ab8dbdde8056Ebae5A6f340720e](https://testnet.bscscan.com/address/0x320106A19C934ab8dbdde8056Ebae5A6f340720e) |
| mAAPL  | [0x0dFa0F08136DA5d28618E7E31A7e24b01a95bB69](https://testnet.bscscan.com/address/0x0dFa0F08136DA5d28618E7E31A7e24b01a95bB69) |
| mGOOGL | [0x56a31ea21862447E3Af9bfe76A45679E44103274](https://testnet.bscscan.com/address/0x56a31ea21862447E3Af9bfe76A45679E44103274) |
| mTSLA  | [0xA2a42F0deB45ca7310a3C02A70fb569d5d5248FA](https://testnet.bscscan.com/address/0xA2a42F0deB45ca7310a3C02A70fb569d5d5248FA) |
| mNFLX  | [0xc6F5e6476958cA81eC8FC68A1ea7c68206b0e501](https://testnet.bscscan.com/address/0xc6F5e6476958cA81eC8FC68A1ea7c68206b0e501) |
| mQQQ   | [0x1Ad3354B2E7C0F7D5A370a03CAf439DD345437a9](https://testnet.bscscan.com/address/0x1Ad3354B2E7C0F7D5A370a03CAf439DD345437a9) |
| mTWTR  | [0x5C4273b1B20112321f0951D0bC2d5eD40c800226](https://testnet.bscscan.com/address/0x5C4273b1B20112321f0951D0bC2d5eD40c800226) |
| mMSFT  | [0xE4f2C30E938c24ee874dfDFAb20fFFBA81323457](https://testnet.bscscan.com/address/0xE4f2C30E938c24ee874dfDFAb20fFFBA81323457) |
| mAMZN  | [0xfBC94545AD2ff3F7B009258FB43F2EAb46744767](https://testnet.bscscan.com/address/0xfBC94545AD2ff3F7B009258FB43F2EAb46744767) |
| mBABA  | [0xFc78bf14Dc997e681dAc4b4D811B45026d04123F](https://testnet.bscscan.com/address/0xFc78bf14Dc997e681dAc4b4D811B45026d04123F) |
| mIAU   | [0xeff3b95faC30230D30F8c8222670A3812D79857B](https://testnet.bscscan.com/address/0xeff3b95faC30230D30F8c8222670A3812D79857B) |
| mSLV   | [0x662DDF725F5BDE9b31BBD16793Fd0c234F67979B](https://testnet.bscscan.com/address/0x662DDF725F5BDE9b31BBD16793Fd0c234F67979B) |
| mUSO   | [0x5D428492846bd05D8137e56Fe806D28606453cbf](https://testnet.bscscan.com/address/0x5D428492846bd05D8137e56Fe806D28606453cbf) |
| mVIXY  | [0x57986628daaDC418E09A2917D6c8b793B7dC1ACD](https://testnet.bscscan.com/address/0x57986628daaDC418E09A2917D6c8b793B7dC1ACD) |
| mFB    | [0x354CA25cf8eB08537f6047e9daF02Eb02222C1D5](https://testnet.bscscan.com/address/0x354CA25cf8eB08537f6047e9daF02Eb02222C1D5) |
| mCOIN  | [0x24fE38158A7550bEd9A451CBeA67dA4BdC920E95](https://testnet.bscscan.com/address/0x24fE38158A7550bEd9A451CBeA67dA4BdC920E95) |

## &#x20;Related Links

### User Guides

* [Terra Bridge User Guide](user-guide/terra-bridge.md)
* [Sending from Mirror on Terra](user-guide/getting-started/sending-tokens.md)
* [Sending from Mirror on ETH & Binance Smart Chain](user-guide/meth-dual-yield/sending-tokens.md)

### For Developers

* [Terra Shuttle Github](https://github.com/terra-project/shuttle)
* [Terra Bridge Web App Github](https://github.com/terra-project/bridge-web-app)
