# HealthKitToFhir Swift Library


The modified HealthKitToFhir Swift Library provides a simple way to create FHIR® Resources from HKObjects.

## Installation

HealthKitToFhir uses **Swift Package Manager** to manage dependencies. It is recommended that you use Xcode 11 or newer to add HealthKitToFhir to your project.

1. Using Xcode 11 go to File > Swift Packages > Add Package Dependency
2. Paste the project URL: https://github.com/Boogy2/healthkit-to-fhir-MSC
3. Click on next and select the project target

## Basic Usage

### Create the factory

Resources are created using "Factory" classes that can be initialized with an optional JSON configuration to provide additional conversion data used to decorate the Resource. In the example below, an observation factory is initialized with no configuration.

```swift
do {
    let  factory = try ObservationFactory()
} catch {
    // Handle errors
}
```

### Use the factory for creating resources

```swift
do {
    let observation = try factory.observation(from: healthKitObject)
} catch {
    // Handle errors
}
```

## Supported conversions

### Observations

Additional observation conversions can be added by providing a custom configuration to the ObservationFactory when it is initialized at runtime.

- HKQuantityTypeIdentifierHeartRate
- HKCorrelationTypeIdentifierBloodPressure
- HKQuantityTypeIdentifierBloodPressureDiastolic
- HKQuantityTypeIdentifierBloodPressureSystolic
- HKQuantityTypeIdentifierStepCount
- HKQuantityTypeIdentifierBloodGlucose
- HKQuantityTypeIdentifierOxygenSaturation
- HKQuantityTypeIdentifierBodyMass
- HKQuantityTypeIdentifierBodyTemperature
- HKQuantityTypeIdentifierRespiratoryRate
- HKQuantityTypeIdentifierHeight
- HKQuantityTypeIdentifierRestingHeartRate
- HKQuantityTypeIdentifierHeartRateVariabilitySDNN
- HKQuantityTypeIdentifierWalkingHeartRateAverage
- HKQuantityTypeIdentifierAppleExerciseTime
- HKQuantityTypeIdentifierAppleStandTime
- HKQuantityTypeIdentifierActiveEnergyBurned
- HKQuantityTypeIdentifierEnvironmentalAudioExposure
- HKQuantityTypeIdentifierDietaryEnergyConsumed

### Devices

The DeviceFactory will extract data provided in HKObject device and sourceRevision properties to create a Device Resource. No configuration is required for this conversion.

## Adding support for new conversions

HealthKitToFhir uses JSON configuration files to provide additional data required to perform conversions from HealthKit HKObjects to FHIR Resources. The [DefaultObservationFactoryConfig.json](Sources/Configuration/DefaultObservationFactoryConfig.json) contains conversion data for the [ObservationFactory](Sources/Factories.ObservationFactory.swift) class to support the types listed above.

The example below shows data used for converting an HKQuantitySample containing a Blood Glucose reading to a FHIR Resource. The HKObject type identifier is used to look up data required to populate the Observation, this includes the code and valueQuantity properties of the Blood Glucose Observation. This "static" data will be "copied" to the Observation during the conversion process, while values like the measurement, date, and identifier will be converted from properties of the HKObject.

```json
"HKQuantityTypeIdentifierBloodGlucose": {
        "code": {
            "coding": [
                {
                    "system": "http://loinc.org",
                    "code": "41653-7",
                    "display": "Glucose Glucometer (BldC) [Mass/Vol]"
                }
            ]
        },
        "valueQuantity": {
            "unit" : "mg/dL",
            "system" : "http://unitsofmeasure.org",
            "code" : "mg/dL"
        }
    }
```

## Supported Conversion 
- HKQuantityTypeIdentifierHeartRate
- HKCorrelationTypeIdentifierBloodPressure
- HKQuantityTypeIdentifierBloodPressureDiastolic
- HKQuantityTypeIdentifierBloodPressureSystolic
- HKQuantityTypeIdentifierStepCount
- HKQuantityTypeIdentifierBloodGlucose
- HKQuantityTypeIdentifierOxygenSaturation
- HKQuantityTypeIdentifierBodyMass
- HKQuantityTypeIdentifierBodyTemperature
- HKQuantityTypeIdentifierRespiratoryRate
- HKQuantityTypeIdentifierHeight
- HKQuantityTypeIdentifierRestingHeartRate
- HKQuantityTypeIdentifierHeartRateVariabilitySDNN
- HKQuantityTypeIdentifierWalkingHeartRateAverage
- HKQuantityTypeIdentifierAppleExerciseTime
- HKQuantityTypeIdentifierAppleStandTime
- HKQuantityTypeIdentifierActiveEnergyBurned
- HKQuantityTypeIdentifierEnvironmentalAudioExposure
- HKQuantityTypeIdentifierDietaryEnergyConsumed
- HKAudiogramSample
- HKElectrocardiogram
- HKWorkout
- HKCharacteristicsTypeIdentifier(HKHealthStore)
- HKCategoryTypeIdentifierSleepAnalysis


## Contributing

This project is a extended clone of:

* [healthkit-to-fhir](https://github.com/Microsoft/healthkit-to-fhir)

FHIR® is the registered trademark of HL7 and is used with the permission of HL7.
