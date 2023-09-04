from bitcoin.rpc import RawProxy
import datetime

# Connect to Bitcoin Core node
rpc = RawProxy()

# Define owner addresses
owner1_address = "owner1_address_here"
owner2_address = "owner2_address_here"

# Define main address and distribution addresses
address1 = "address1_here"
address2 = "address2_here"
address3 = "address3_here"
address4 = "address4_here"
address5 = "address5_here"
address6 = "address6_here"  # The unlock destination address

# Define the locking time for address1 (9th to 10th of each month)
locking_time_start = datetime.datetime(datetime.datetime.utcnow().year, datetime.datetime.utcnow().month, 9).timestamp()
locking_time_end = datetime.datetime(datetime.datetime.utcnow().year, datetime.datetime.utcnow().month, 10).timestamp()

# Unlock and distribute funds (automatically triggered on the 11th day of each month)
def unlock_and_distribute():
    current_date = datetime.datetime.utcnow()
    if current_date.day == 11:
        if locking_time_start <= current_date.timestamp() <= locking_time_end:
            balance = rpc.getreceivedbyaddress(address1, 0)
            distribution_amount = balance / 4  # 25% of the balance for each address
            for address in [address2, address3, address4, address5]:
                rpc.sendtoaddress(address, distribution_amount)
        else:
            print("Address1 is locked during this time period.")
    else:
        if locking_time_start <= current_date.timestamp() <= locking_time_end:
            # Check multisignature condition and send transaction to address6
            if owner1_address in rpc.listunspent() and owner2_address in rpc.listunspent():
                multisig_inputs = [
                    {"txid": tx["txid"], "vout": tx["vout"]} for tx in rpc.listunspent() if tx["address"] in [owner1_address, owner2_address]
                ]
                output_transaction = [{"address": address6, "amount": 0.01}]  # Amount for demonstration
                raw_transaction = rpc.createrawtransaction(multisig_inputs, output_transaction)
                signed_transaction = rpc.signrawtransactionwithwallet(raw_transaction)
                rpc.sendrawtransaction(signed_transaction["hex"])
        else:
            print("Address1 is locked during this time period.")

# Call unlock_and_distribute function
unlock_and_distribute()
