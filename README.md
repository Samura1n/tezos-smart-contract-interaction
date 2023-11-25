# tezos-smart-contract-interaction 
The tezos-smart-contract-interaction script simplifies interactions with smart contracts on the Tezos blockchain, offering a user-friendly interface for executing custom transactions and querying contract data.
Leveraging the PyTezos library, this script enables users to originate smart contracts, invoke contract entry points, and retrieve contract storage information. It serves as a practical foundation for developers and enthusiasts engaging with Tezos smart contracts.

from pytezos import ContractInterface, pytezos

class TezosSmartContractInteractionScript:
    def __init__(self, node_url, private_key):
        self.tezos = pytezos.using(node_url).using(key=private_key)

    def originate_smart_contract(self, contract_path, initial_storage, fee=1000000, gas_limit=100000, storage_limit=10000):
        # Originate a smart contract on the Tezos blockchain
        contract = ContractInterface.from_file(contract_path)
        operation = self.tezos.originate(contract).using(
            shell=self.tezos.shell,
            key=self.tezos.key,
            amount=0,
            balance=0,
            fee=fee,
            gas_limit=gas_limit,
            storage_limit=storage_limit,
            parameters=initial_storage
        ).autofill().sign().inject()

        print(f"Smart Contract originated. Operation hash: {operation['hash']}")

    def invoke_contract_entry_point(self, contract_address, entry_point, parameters, fee=1000000, gas_limit=100000, storage_limit=10000):
        # Invoke a specific entry point of a smart contract
        contract = self.tezos.contract(contract_address)
        operation = contract.call(entry_point).using(
            key=self.tezos.key,
            amount=0,
            balance=0,
            fee=fee,
            gas_limit=gas_limit,
            storage_limit=storage_limit,
            parameters=parameters
        ).autofill().sign().inject()

        print(f"Contract entry point invoked. Operation hash: {operation['hash']}")

    def get_contract_storage(self, contract_address):
        # Retrieve storage information of a smart contract
        contract = self.tezos.contract(contract_address)
        storage_data = contract.storage()

        print(f"Smart Contract Storage Information:")
        print(storage_data)
        print("-" * 30)

# Example Usage
tezos_node_url = "https://mainnet.smartpy.io/"
your_private_key = "your_private_key_here"
smart_contract_path = "path/to/your/smart_contract.tz"

tezos_script = TezosSmartContractInteractionScript(tezos_node_url, your_private_key)

# Originate a smart contract
initial_storage = {"parameter": "initial_value"}
tezos_script.originate_smart_contract(smart_contract_path, initial_storage)

# Invoke a contract entry point
contract_address = "your_contract_address_here"
entry_point = "your_entry_point"
parameters = {"parameter": "new_value"}
tezos_script.invoke_contract_entry_point(contract_address, entry_point, parameters)

# Retrieve contract storage
tezos_script.get_contract_storage(contract_address)

This script simplifies smart contract interactions on the Tezos blockchain, providing an easy-to-use interface for originating contracts, invoking entry points, and retrieving contract storage. It is designed to assist developers and enthusiasts in working with Tezos smart contracts.
