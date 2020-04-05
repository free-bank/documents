mobile banking

create a product to define transaction/charge/profit rules for the accounts created under it
configure gls to reflect accounting

create a user [auth info]
create a customer [personl info details]
create an account [account to transact]
trnasactions:
put some money in the account
withdraw some cash from the account
transfer some money from your account to another account

components:
authentication -  spring security - oauth2/key cloak
gateway
customer
 - model: customer
   cid
   first name
   last name
   address
   dob
   job details

product
 - model: product
   pid
   name
   profit rate
   yearly charge
   monthly charge
   per transaction withdraw charge   
   maximum amount withdrawal in a month to make profit
   maximum number of withdrawal in a month to make profit
   
   
gl
 -model: glaccount
  glaccid
  description
  parentid

account details
 - model: account
   accid
   profit rate
   yearly charge
   monthly charge
   numberofwithdraw transactions in a month
   number of withdraw amount in a month
   status
   balance

account transaction
 - model:
   TransactionAccount
     accid
     balance
     withdrawcharge:
     status: ready/suspended
     transactions : list of transaction
        trid
        type debit or credit
        amount
        time
        


scenario - how to do transactions?
transfer
0. request comes and gets logged (in something like kafka/kinesis)
2. transaction service picks the job
a. get account transaction rules and status from account where there will be credit
b. get account transaction rules, status and balance from debit account 
c. verify whether transaction can happen based on available data from the above info
d. if yes, update debit account conditionally
g. then update credit account
h. if g fails, then reverse d
i. create a log as successful request completion (something like kafka/kinesis)
j. picks up the success status and 
     - send update info to users
     - update account details for balance
     - update gl transactions
k. on failure log locally
l. some other elk like mechanism will grab the log and create failure message (if that was failed) on kafka/kinesis
