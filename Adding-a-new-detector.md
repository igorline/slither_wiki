Slither's plugin architecture lets you integrate new detectors that run from the command line.

## Detector Skeleton

The skeleton for a detector is:

```python
from slither.detectors.abstract_detector import AbstractDetector, DetectorClassification


class Skeleton(AbstractDetector):
    """
    Documentation
    """

    ARGUMENT = 'mydetector' # slither will launch the detector with slither.py --detect mydetector
    HELP = 'Help printed by slither'
    IMPACT = DetectorClassification.HIGH
    CONFIDENCE = DetectorClassification.HIGH

    WIKI = ''

    WIKI_TITLE = ''
    WIKI_DESCRIPTION = ''
    WIKI_EXPLOIT_SCENARIO = ''
    WIKI_RECOMMENDATION = ''

    def _detect(self):
        info = 'This is an example!'
        res = self.generate_result(info)

        return [res]
```

- `ARGUMENT` lets you run the detector from the command line
- `HELP` is the information printed from the command line
- `IMPACT` indicates the impact of the issue. Allowed values are:
  - `DetectorClassification.INFORMATIONAL`: printed in green
  - `DetectorClassification.LOW`: printed in green
  - `DetectorClassification.MEDIUM`: printed in yellow
  - `DetectorClassification.HIGH`: printed in red
- `CONFIDENCE` indicates your confidence in the analysis. Allowed values are:
  - `DetectorClassification.LOW`
  - `DetectorClassification.MEDIUM`
  - `DetectorClassification.HIGH`
- `WIKI` constants are used to generate automatically the documentation.

`_detect()` needs to return a list of findings. A finding is an element generated with `self.generate_result(info)`, where `info`  is a list of text or contract's object (contract, function, node, ...)

An `AbstractDetector` object has the `slither` attribute, which returns the current `Slither` object.

## Integration

You can integrate your detector into Slither by:
- Adding it in [slither/detectors/all_detectors.py](https://github.com/trailofbits/slither/blob/5cc07a3608a154a2fa022c3e064af4e699d63dda/slither/detectors/all_detectors.py)
- or, by creating a plugin package (see the [skeleton example](https://github.com/trailofbits/slither/tree/8f91c801c0bb903990c4fc9fa30611f157c6b0f9/plugin_example)).

### Test the detector
If you want to add your detector to [trailofbits/slither](https://github.com/trailofbits/slither), create a unit-test in [tests](https://github.com/trailofbits/slither/tree/master/tests) and update [scripts/travis_test_4.sh](https://github.com/trailofbits/slither/blob/master/scripts/travis_test_4.sh#L92) to run the unit-test automatically.

[deepdiff](https://github.com/seperman/deepdiff) is needed to run `travis_test.sh`:
```
pip install deepdiff
```

## Example
[backdoor.py](https://github.com/trailofbits/slither/blob/5cc07a3608a154a2fa022c3e064af4e699d63dda/slither/detectors/examples/backdoor.py) will detect any function with `backdoor` in its name.
