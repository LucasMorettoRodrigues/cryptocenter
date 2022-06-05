<template>
    <main class="main-container">

        <Modal title="Buy" @close="handleClose" :open="showModal">
            <form @submit="handleBuy">
                <SelectComponent label="Name" v-model="name" :options="this.data.map(i => i.name)" />
                <InputComponent label="Amount" type="number" step="any" v-model="amount" />
                <div class="btn-container">
                    <ButtonComponent text="Buy" type="submit" />
                </div>
            </form>
        </Modal>

        <Modal title="Buy" @close="handleClose" :open="showBuyModal">
            <form @submit="handleBuy">
                <SelectComponent label="Name" v-model="name" :options="this.data.map(i => i.name)" />
                <InputComponent label="Amount" type="number" step="any" v-model="amount" />
                <div class="btn-container">
                    <ButtonComponent text="Buy" type="submit" />
                </div>
            </form>
        </Modal>

        <Modal title="Sell" @close="handleClose" :open="showSellModal">
            <form @submit="handleSell">
                <SelectComponent label="Name" v-model="name" :options="this.data.map(i => i.name)" />
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

            <div v-if="this.data" class="flex">
                <TotalCard :wallet="this.wallet" :data="this.data" />
                <div class="right">
                    <PieChart :serie="this.wallet.map(item => (item.balance * this.data.find(i => i.name ===
                    item.name).quote.USD.price))" :label="this.wallet.map(item => item.name)" />
                </div>
            </div>

            <div class="wallet">
                <ul class="item-info flex align-center">
                    <li>Name</li>
                    <li>Quantity</li>
                    <li>Balance (USD)</li>
                    <li></li>
                </ul>
                <transition-group tag="ul" class="wallet-list" name="wallet-list">
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
                </transition-group>
            </div>

        </section>
    </main>
</template>

<script>
import PieChart from '../components/PieChart.vue';
import TotalCard from '../components/TotalCard.vue';
import InputComponent from '../components/InputComponent.vue';
import SelectComponent from '../components/SelectComponent.vue';
import Modal from '../components/Modal.vue';
import ButtonComponent from '../components/ButtonComponent.vue';

export default {
    name: "WalletView",
    components: {
        PieChart,
        TotalCard,
        InputComponent,
        SelectComponent,
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
            formatter: new Intl.NumberFormat('en-US', {
                style: 'currency',
                currency: 'USD',
            }),
            wallet: [
                { name: 'Bitcoin', balance: 0.23 },
                { name: 'USD Coin', balance: 4000.00 },
                { name: 'Ethereum', balance: 1.02 },
                { name: 'Cardano', balance: 2500 },
                { name: 'BNB', balance: 3.55 }
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
                console.log('ak')
                // Add coin to wallet
                this.wallet.push({ name: this.name, balance: 0 })
                this.balance = 0
            } else {
                this.balance = coin.balance
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
            this.showBuyModal = true
        },
        handleOpenSellModal(coin) {
            this.name = coin.name
            this.balance = coin.balance
            this.showSellModal = true
        }
    },
    inject: ['getData'],
    computed: {
        data() {
            return this.getData()
        }
    }
}
</script>

<style scoped>
.header-container {
    margin: 20px 0;
}

.right,
.left {
    flex: 1;
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

.wallet-list-enter-active {
    animation: list .5s ease-out;
}

.wallet-list-leave-active {
    animation: list .5s ease-in reverse;
}

@keyframes list {
    0% {
        opacity: 0;
        transform: translateX(-30px);
    }

    100% {
        opacity: 1;
        transform: translateX(0);
    }
}
</style>
