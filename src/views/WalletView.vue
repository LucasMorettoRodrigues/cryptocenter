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

            <div v-if="this.data" class="top-container flex">
                <div class="top-container-item">
                    <div class="total-box">
                        <h2>Total in Assets:</h2>
                        <h1>{{ formatter.format(this.wallet.reduce((s, i) => s + (i.balance * this.data.find(j => j.name
                                ===
                                i.name).quote.USD.price), 0))
                        }}</h1>
                    </div>
                </div>
                <div class="top-container-item">
                    <PieChart :serie="this.wallet.map(item => (item.balance * this.data.find(i => i.name ===
                    item.name).quote.USD.price))" :label="this.wallet.map(item => item.name)" />
                </div>
            </div>

            <div v-else="this.data" class="top-container flex">
                <div class="top-container-item">
                </div>
                <div class="top-container-item">
                    <PieChart :serie="[1]" :label="['One']" />
                </div>
            </div>

            <div class="wallet">
                <ul class="item-info flex align-center">
                    <li>Name</li>
                    <li>Quantity</li>
                    <li>Balance (USD)</li>
                    <li></li>
                </ul>
                <ul class="wallet-list">
                    <li class="item-list" v-for="(item, index) in wallet" :key="index">
                        <ul class="item-info flex align-center">
                            <li class="name">{{ item.name }}</li>
                            <li class="balance">{{ item.balance }}</li>
                            <li class="balance">{{ this.data && formatter.format(this.data.find(i => i.name ===
                                    item.name).quote.USD.price *
                                    item.balance)
                            }}</li>
                            <li class="flex">
                                <button class="btn green" @click="() => handleOpenBuyModal(item)">Buy</button>
                                <button class="btn red" @click="() => handleOpenSellModal(item)">Sell</button>
                            </li>
                        </ul>
                    </li>
                </ul>
            </div>

        </section>

    </main>
</template>

<script>
import PieChart from '../components/PieChart.vue';
import InputComponent from '../components/InputComponent.vue';
import Modal from '../components/Modal.vue';
import ButtonComponent from '../components/ButtonComponent.vue';
import axios from 'axios';

export default {
    name: "WalletView",
    components: {
        PieChart,
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
                { name: 'Bitcoin', balance: 1.023 },
                { name: 'USD Coin', balance: 14000.00 },
                { name: 'Ethereum', balance: 8 },
                { name: 'Cardano', balance: 10000 }
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

            console.log(this.wallet)

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
                    ? { ...item, balance: Number((this.balance - this.amount).toFixed(2)) }
                    : item
                )

                console.log(this.wallet)
            }

            this.handleClose()
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

            this.cleanFields()
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
        },
        async getData() {
            try {
                const data = await axios.get("http://localhost:3000/api/v1/cryptocurrency/listings/latest", {
                    headers: {
                        'X-CMC_PRO_API_KEY': '000470f2-3058-4f7a-88a8-b944668f4e89'
                    }
                })
                this.data = data.data.data
                console.log(this.data.find(i => i.name === 'Bitcoin').quote.USD.price)
            } catch (error) {
                console.log(error)
            }
        }
    },
    mounted() {
        this.getData()
    }
}
</script>

<style scoped>
.header-container {
    margin: 20px 0;
}

.top-container {
    justify-content: space-evenly;
}

.top-container-item {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
}

.total-box {
    background-color: rgb(233, 233, 233);
    padding: 50px 80px;
    border-radius: 5px;
}

.total-box h2 {
    margin-bottom: 20px;
    font-size: 26px;
    color: #555;
}

.total-box h1 {
    font-size: 40px;
    color: #111;
}

.wallet {
    padding: 20px 0;
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
    margin-left: 10px;
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
