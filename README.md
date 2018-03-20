## Scuba diving algorithms, formulas, calculations and device management

#### Table of Contents

* [Getting Started](#getting-started)
    * [Dependencies](#dependencies)
    * [Developing](#developing)
    * [Testing](#testing)
    * [Documentation](#documentation)
    * [Use Case](#uses)
* [Credits](#credits)
* [License](#license)

### Getting Started

## Dependencies

What you need to run this seed:
* `node` and `npm` (Use [NVM](https://github.com/creationix/nvm))
* Ensure you're running Node (`v4.1.x`+) and NPM (`2.14.x`+)

It will start a local demo server using `webpack-dev-server` which will watch, build (in-memory), and reload for you. The port will be displayed to you as `http://localhost:8080`.

## Developing

### Build files

* single run: `npm run build`
* build files and watch: `npm run watch`

## Testing

#### 1. Unit Tests

* single run: `npm test`
* live mode (TDD style): `npm run test-watch`

#### 2. End-to-End Tests (aka. e2e, integration)

* single run:
  * in a tab, *if not already running!*: `npm start`
  * in a new tab: `npm run webdriver-start`
  * in another new tab: `npm run e2e`
* interactive mode:
  * instead of the last command above, you can run: `npm run e2e-live`
  * when debugging or first writing test suites, you may find it helpful to try out Protractor commands without starting up the entire test suite. You can do this with the element explorer.
  * you can learn more about [Protractor Interactive Mode here](https://github.com/angular/protractor/blob/master/docs/debugging.md#testing-out-protractor-interactively)

## Documentation

You can generate api docs (using [TypeDoc](http://typedoc.org/)) for your code with the following:
```bash
npm run docs
```

## Uses
var dive = require('scuba-dive');
// or <script type="text/javascript" src="scuba-dive.js"></script>
dive.feetToMeters(); // 0.3048
dive.feetToMeters(33); // 10.0584
dive.metersToFeet(10); // 3.28084

// check defaults or change them
dive.gravitySamples.current(); // 9.80665 m/s2
dive.gravitySamples.current(9.8);
dive.gravitySamples.current(); // 9.8 m/s2
dive.surfacePressureSamples.current(); // 1 bar
dive.surfacePressureSamples.current(1);
dive.liquidSamples.fresh.density(); // 1000 kg/m3 (1 ton of a m^3)
dive.liquidSamples.salt.density(); // 1030 kg/m3
dive.liquidSamples.mercury.density(); // 13595.1 kg/m3
dive.constants.vapourPressure.lungsBreathing.current(); // 0.056713577314554675 bars (commonly seen in buhlmann deco)
dive.constants.altitudePressure.current(); // 1 bar (for sea level)
dive.constants.altitudePressure.current(0.6600); // (approx. 3000 meters above sea level)
dive.constants.altitudePressure.current(1); // change back to default at sea-level

// calculate dac, sac and rmv rate
dive.dac(2500, 1300, 50); // 24 psi/min
dive.sac(24, 10); // 12.022841322028032 psi/min given 24 psi/min dac rate
dive.rmv(25, 80, 3000); // 0.67 cuft/min given 25 psi/min sac rate

// calculate various depth of 1 cubic meter volume of liquid in atm or bars
dive.depthInMetersToBars(10); // 2.0094000000000003 bar absolute
dive.depthInMetersToAtm(10); // 1.9962003454231434 atm
dive.atmToDepthInMeters(1); // 10.038141470180305 meters
dive.barToDepthInMeters(1); // 9.906875371507827 meters

// calculate depth meters for all calculations above but in fresh water
dive.depthInMetersToBars(10, true); // 1.98 bar absolute (1 above)
dive.depthInMetersToAtm(10, true); // 1.9671848013816926 atm absolute (1 above)
dive.atmToDepthInMeters(1, true); // 10.339285714285714 meters
dive.barToDepthInMeters(1, true); // 10.204081632653061 meters

// calculating partial pressure of a particular gas component mixture
// calculate partial pressure for gas mixture that is 79% nitrogen (N2) and 21% oxygen (O2)
// at 10 meters below sea level, what is partial pressure of each gas?
var p = dive.depthInMetersToBars(10); // 2.0094000000000003 bar absolute
dive.partialPressure(p, 0.79); // 1.5874260000000002 bar absolute
dive.partialPressure(p, 0.21); // 0.42197400000000007 bar absolute

// you can also use the helper function to calculate from depth in meters
dive.partialPressureAtDepth(10, 0.79); // 1.5874260000000002 bar absolute
dive.partialPressureAtDepth(10, 0.21); // 0.42197400000000007 bar absolute

// calculate partial pressures for above but in fresh water
dive.partialPressureAtDepth(10, 0.79, true); // 1.5642 bar absolute
dive.partialPressureAtDepth(10, 0.21, true); // 0.4158 bar absolute

// calculate water vapour pressure at a given temperature (degrees celcius)
// use the conversion tools to calculate in terms of bars
var mmHg = dive.waterVapourPressure(35.2); // 42.538675172399344 mmHg
var pascals = dive.mmHgToPascal(mmHg); // 5671.357731455468 Pascal
dive.pascalToBar(pascals); // 0.056713577314554675 bar
dive.waterVapourPressureInBars(35.2); // 0.056713577314554675 bar
// notice the 0.0567 bar above? we can plug that into the Buhlmann Decompression Algorithm

// calculate maximum operating depth using AIR (21% Oxygen) at 1.4 bars
dive.maxOperatingDepth(1.4, 0.21); // 56.10089197613197 meters
// calculate maximum operating depth using AIR (21% Oxygen) at 1.4 bars in fresh water
dive.maxOperatingDepth(1.4, 0.21, true); // 57.78391873541593 meters

// calculate the narcotic equivalent depth for breathing a gas mixture
// that is 12% Oxygen, 38% Nitrogen and 50% Helium at 100m
dive.equivNarcoticDepth(0.12, 0.38, 0.50, 100); // 56.882888936481194 meters air
dive.equivNarcoticDepth(0.12, 0.38, 0.50, 100, true); // 56.766365121623096 meters air (in fresh water)

// depth change in bars per minute (e.g. 0 to 10 meters in 30 seconds)
dive.depthChangeInBarsPerMinute(0, 10, 0.5); // 2.0201699 bars per minute (in salt water)
dive.depthChangeInBarsPerMinute(0, 10, 0.5, true); // 1.9613300000000002 bars per minute (in fresh water)

// gas rate in bars per minute (e.g. 0 to 10 meters in 30 seconds with 79% Nitrogen)
dive.gasRateInBarsPerMinute(0, 10, 0.5, 0.79); // 1.595934221 bars per minute (in salt water)
dive.gasRateInBarsPerMinute(0, 10, 0.5, 0.79,, true); // 1.5494507 bars per minute (in fresh water)

// gas breathing pressure in bars accounting for water vapour pressure in the lungs
// calculate the pressure at 10 meters breathing 79% N2
dive.gasPressureBreathingInBars(10, 0.79); // 0.7531633844215019 bars (in salt water)
dive.gasPressureBreathingInBars(10, 0.79, true); // 0.729921623921502 bars (in fresh water)

// instantaneous equation (exposed at 20 meters with AIR for 40 minutes)
var pGas = dive.gasPressureBreathingInBars(20, 0.79);
dive.instantaneousEquation(0.79, pGas, 40, 4.0); // 1.5940936555866738 bar

// buhlmann deco algorithm (*this needs review*)
var buhlmannDeco = dive.deco.buhlmann();
var newPlan = new buhlmannDeco.plan(buhlmannDeco.ZH16ATissues); // 1 abs pressure in fresh water
newPlan.addBottomGas("2135", 0.21, 0.35);
newPlan.addDecoGas("50%", 0.5, 0.0);
newPlan.addDepthChange(0, 50, "2135", 5);
newPlan.addFlat(50, "2135", 25);
var decoPlan = plan.calculateDecompression(false, 0.2, 0.8, 1.6, 30); //gradientFactorLow = 0.2, gradientFactorHigh=0.8, deco ppO2 = 1.6, and max END allowed: 30 meters.

//No-Deco limit for a gas at a depth
var buhlmann = dive.deco.buhlmann();
var newPlan = new buhlmann.plan(buhlmann.ZH16BTissues);
newPlan.addBottomGas("air", 0.21, 0.0);
var gradientFactor = 1.5; //This was choosen to closely match PADI dive tables.
newPlan.ndl(dive.feetToMeters(140), "air", gradientFactor), "NDL for 140 feet should be close to 7 minutes");

## Credits

* Project setup based on [angular2-webpack](https://github.com/preboot/angular2-webpack)
* Testing strategies found from [Angular —  Unit Testing recipes by Gerard Sans](https://medium.com/google-developer-experts/angular-2-unit-testing-with-jasmine-defe20421584)

# License

[MIT](/LICENSE)
