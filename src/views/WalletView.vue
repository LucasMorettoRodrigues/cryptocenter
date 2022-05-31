<template>
    <main class="main-container">

        <Modal v-if="showModal" title="Buy" @close="handleClose">
            <form @submit="handleBuy">
                <InputComponent label="Name" type="string" v-model="name" />
                <InputComponent label="Amount" type="number" step="any" v-model="amount" />
                <div class="btn-container">
                    <ButtonComponent text="Buy" type="submit" />
                </div>
            </form>
        </Modal>

        <Modal v-if="showBuyModal" title="Buy" @close="handleClose">
            <form @submit="handleBuy">
                <InputComponent label="Name" type="string" v-model="name" />
                <InputComponent label="Amount" type="number" step="any" v-model="amount" />
                <div class="btn-container">
                    <ButtonComponent text="Buy" type="submit" />
                </div>
            </form>
        </Modal>

        <Modal v-if="showSellModal" title="Sell" @close="handleClose">
            <form @submit="handleSell">
                <InputComponent label="Name" type="string" v-model="name" />
                <InputComponent label="Amount" type="number" step="any" v-model="amount" />
                <div class="btn-container">
                    <ButtonComponent text="Sell" type="submit" />
                </div>
            </form>
        </Modal>

        <section class="wallet-section">
            <div class="flex space-between header-container">
                <h1>My Wallet</h1>
                <ButtonComponent text='Buy New Asset' @click="showModal = true" />
            </div>
            <ul class="item-info flex align-center">
                <li>Name</li>
                <li>% of Balance</li>
                <li>Balance</li>
                <li></li>
            </ul>
            <ul class="wallet-list">
                <li class="item-list" v-for="(item, index) in wallet" :key="index">
                    <ul class="item-info flex align-center">
                        <li class="name">{{ item.name }}</li>
                        <li class="percentage">{{ (item.balance / wallet.reduce((sum, item) => sum + item.balance,
                                0) * 100).toFixed(2)
                        }} % </li>
                        <li class="balance">{{ formatter.format(item.balance) }}</li>
                        <li class="flex">
                            <button class="btn green" @click="() => handleOpenBuyModal(item)">Buy</button>
                            <button class="btn red" @click="() => handleOpenSellModal(item)">Sell</button>
                        </li>
                    </ul>
                </li>
            </ul>
        </section>

    </main>
</template>

<script>
import InputComponent from '../components/InputComponent.vue';
import Modal from '../components/Modal.vue';
import ButtonComponent from '../components/ButtonComponent.vue';

export default {
    name: "HomeView",
    components: {
        InputComponent,
        Modal,
        ButtonComponent,
    },
    data() {
        return {
            showModal: false,
            showBuyModal: false,
            showSellModal: false,
            selected: '',
            name: '',
            balance: 0,
            amount: 0,
            data: '',
            formatter: new Intl.NumberFormat('en-US', {
                style: 'currency',
                currency: 'USD',
            }),
            wallet: [
                { name: 'Bitcoin', balance: 15201.01 },
                { name: 'USDCoin', balance: 10000.00 },
                { name: 'Ethereum', balance: 1200.52 },
                { name: 'Cardano', balance: 600.95 }
            ]
        }
    },
    methods: {
        handleBuy(e) {
            e.preventDefault()
            if (!this.name || !this.amount) {
                return console.log('Missing fields.')
            }

            if (this.amount <= 0) {
                return console.log('Invalid Amount.')
            }

            const coin = this.wallet.find(item => item.name === this.name)

            if (!coin) {
                // Add coin to wallet
                this.wallet.push({ name: this.name, balance: 0 })
            }

            // Update coin balance
            this.wallet = this.wallet.map(item => item.name === this.name
                ? { ...item, balance: Number(this.balance) + Number(this.amount) }
                : item
            )

            this.handleClose()
            this.cleanFields()
        },
        handleSell(e) {
            e.preventDefault()
            console.log(this.balance, this.amount)
            if (!this.name || !this.balance || !this.amount) {
                return console.log('Missing fields.')
            }

            if (this.amount > this.balance) {
                return console.log('Insuficcient funds.', this.amount, this.balance)
            }

            if (this.amount == this.balance) {
                // Remove coin from wallet
                this.wallet = this.wallet.filter(item => item.name !== this.name)
            } else {
                // Update coin balance
                this.wallet = this.wallet.map(item => item.name === this.name
                    ? { ...item, balance: (this.balance - this.amount).toFixed(2) }
                    : item
                )
            }

            this.handleClose()
            this.cleanFields()
        },
        cleanFields() {
            this.name = ''
            this.balance = 0
            this.amount = 0
        },
        handleClose() {
            this.showModal = false
            this.showBuyModal = false
            this.showSellModal = false
        },
        changeName(event) {
            this.name = event
        },
        changeAmount(event) {
            this.amount = event
        },
        changeBalance(event) {
            this.balance = event
        },
        handleOpenBuyModal(coin) {
            this.name = coin.name
            this.balance = coin.balance
            this.showBuyModal = true
        },
        handleOpenSellModal(coin) {
            this.name = coin.name
            this.balance = coin.balance
            this.showSellModal = true
        },
        teste(event) {
            console.log(event)
        }
    }
}
</script>

<style scoped>
.header-container {
    margin: 20px 0;
}

.wallet-section {
    padding: 20px 0;
    min-height: 78vh;
}

.wallet-list {
    padding: 0;
}

.item-list {
    background-color: rgb(233, 233, 233);
    height: 50px;
    margin: 3px 0;
    border-radius: 5px;
}

.item-info {
    padding: 0;
    height: 100%;
}

.item-info li {
    flex: 1;
    padding: 10px;
}

.item-info li:last-child {
    text-align: right;
    flex: 0;
    min-width: 150px;
}

.item-info .name {
    font-size: 14px;
    font-weight: bold;
}

.item-info .percentage {
    font-size: 14px;
    font-weight: 300;
}

.item-info .balance {
    font-size: 14px;
    font-weight: 500;
}

.btn {
    margin-right: 10px;
    padding: 8px 15px;
    border: none;
    text-align: right;
    font-weight: bold;
    color: white;
    cursor: pointer;
    border-radius: 5px;
    opacity: 0.8;
    transition: ease-in-out 0.2s;
}

.btn:hover {
    opacity: 1;
}

.btn.green {
    color: green;
}

.btn.green:hover {
    background-color: green;
    color: white;
}

.btn.red {
    color: red;
}

.btn.red:hover {
    background-color: red;
    color: white;
}

.btn-container {
    margin-top: 5px;
}
</style>
