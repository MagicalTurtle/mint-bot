# Contract Minting Bot

A simple bot which allows automated NFT minting via [Flashbots](https://docs.flashbots.net/).

## Gas Prices

This bot uses [Etherchain](https://etherchain.org/tools/gasnow) to estimate gas prices

## How to use the bot

The bot uses toml files for configuration. Please see `example.toml` for a basic template.

It is also advisable to use a **Burner Wallet** when using this bot. **NEVER USE YOUR COLD WALLET**.

1. Generate an encrypted private key file by running the application and selecting `Encrypt private key` then folllowing the prompts.
2. Copy `example.toml` into another file. (e.g `chubbicorns.toml`)
3. Generate a flashbot signer key (This only needs to be done once) by running the application and selecting `Generate Flashbot Signer`
4. Edit the configurations inside the new file.
5. Run the bot and select the configuration you would like to use.

## Examples

This section will go through common contract scenarios and what configuration values should be set.

### Basic Free Mint Contract

```toml
contract_address = "0x0000000000000000000000000000000000000000"
function = "mint(uint256)"
arguments = [{ type = "uint256", value = 2 }]
value = 0
```

### Paid Mint Contract

`value` will need to be set to `price per nft in eth * total amount minting`

```toml
contract_address = "0x0000000000000000000000000000000000000000"
function = "mint(uint256)"
arguments = [{ type = "uint256", value = 2 }] # Minting 2 nfts with a price of 0.1eth each
value = 0.2 # 0.2eth is required for the single transaction
```

### Automatic Gas

The bot uses a simple gas oracle to calculate gas.

```toml
gas_preset = "instant" # You can use slow, standard, fast, instant
gas_limit = 250_000
```

### Custom Gas

If you would like to set your own gas use the following:

```toml
gas_preset = "custom"
gas_fee = 50 # Change to your preferred gas fee here
priority_fee = 2 # Change to your preferred priority fee here
gas_limit = 250_000
```

### Multi mint

If a contract allows multiple mints but limits the mints per transaction then you can configure values like so:

```toml
contract_address = "0x0000000000000000000000000000000000000000"
function = "mint(uint256)" # Mint only allows 5 per tx and 10 max per wallet
arguments = [{ type = "uint256", value = 5 }]
value = 0.2 # The cost of mint PER transaction
transaction_count = 2 # We need to run the function above twice to mint all 10 NFTs
```

### Delayed start time

If a contract has a set time for public mint then you can do the following:

```toml
# The public minting time in unix epoch seconds
# https://www.epochconverter.com/
start_time = 1654494482
```

### Testing Configuration

If you would like to test the bot without submitting bundles then set the following value:

```toml
dry_run = true
```

## Disclaimer

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
