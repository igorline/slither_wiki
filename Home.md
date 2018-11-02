## Adding a detector

1. Review the guide to [adding a detector](Adding-a-new-detector)
2. Reference the complete [API documentation](API-examples)
3. Reference the complete [SlithIR definition](SlithIR)

## Using the printers

Slither includes a set of "printers" to help visualize information in smart contracts. Review the [documentation on available printers](Printer-documentation).

## Bounties

We're happy to offer bounties for contributions to Slither! Please review [Gitcoin](https://gitcoin.co/profile/trailofbits) for a list of available bounties.

Upon any new release, Trail of Bits may consider sending a bounty payment to any PR that was not covered by Gitcoin. In these cases, we will send an automated payment to the email in your `git log` via PayPal.

In general, we are interested in and would like to see contributions of:
* Detector modules for new conditions and vulnerability patterns
* Enhancements to existing detector modules to improve confidence in the analysis
* Enhancements and refinements to the SlithIR intermediate representation
* Architectural and code quality improvements of any kind
* Bugfixes of any kind
* 3rd-party writeups and blogs that demonstrate advanced usage of Slither
* Help with any issues labeled "good first issue" or "help wanted"

## Troubleshooting

### FileNotFoundError: [Errno 2] No such file or directory: 'solc'

You may have installed solcjs, not solc. Slither requires solc. Install one of the [binary packages](https://solidity.readthedocs.io/en/v0.4.21/installing-solidity.html#binary-packages), e.g., via apt-get, and Slither will work.

### pip: slither-analyzer requires Python '>=3.6' but the running Python is 2.7.15

Slither requires Python3. If you are on macOS, run `brew install python3` then `pip3 install slither-analyzer`.