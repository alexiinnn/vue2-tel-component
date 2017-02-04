<template>
    <form v-on:click.preventDefault()  class="form form-tel"  id="tel">
        <div class="form__title">Phone number</div>
        <div class="tel">
            <div class="dropdown" v-bind:class="{open: showDropdown}">
                <div class="button dropdown__button" v-on:click="toggle($event)">
                    <span class="flag" v-bind:class="flagSelected"></span>
                    <span>{{ codeSelected }}</span>
                    <span class="dropdown__arrow">&#x25BC;</span>
                </div>
                <ul class="dropdown__menu">
                    <li v-for="item in buckets" v-on:click="changeCountry(item)" class="dropdown__item">
                        <span class="flag" v-bind:class='"flag"+item.domain'></span>
                        <span>{{item.country}}</span>
                        <span class="dropdown__item__code">+{{item.code}}</span>
                    </li>
                </ul>
            </div>
            <input type="hidden" v-bind:value="codeSelected">
            <div class="tel_number__wrapper">
                <input type="tel" class="tel_number" name="telNumber" placeholder="Your number"
                       v-on:keyup="checkNumber(numberForCheck)"
                       v-model="numberForCheck"
                >
                <div class="tel_number__warning" v-bind:class="{tel_number__warningShow: showWarning}">
                    <span class="tel_number__warning__triangle">▲</span>
                    <span class="tel_number__warning__message">Numbers only, please</span>
                </div>
            </div>
        </div>
        <div class="form-handle">
            <div class="form__message__wrapper">
                <div class="form__message" v-bind:class="{form__messageOpen: sendSuccesss}">{{sendMessage}}</div>
            </div>
            <button type="submit" class="button button__send" form="tel2" v-on:click="send($event)" v-bind:disabled="!sendActive">&#x2714;</button>
        </div>
    </form>
</template>

<script>
    export default {
        name: 'tel',
        data() {
            return {
                showDropdown: false,
                sendSuccesss:'',
                sendMessage: '',
                flagSelected: 'flagempty',
                sendActive: true,
                showWarning: false,
                buckets: [
                    {country: 'Belarus', code: 375, domain: 'by'},
                    {country: 'Netherlands', code: 31, domain: 'nl'},
                    {country: 'Poland', code: 48, domain: 'pl'},
                    {country: 'Thailand', code: 66, domain: 'th'},
                    {country: 'Vietnam', code: 84, domain: 'vn'}
                ],
                codeSelected: 'Choose',
                domainSelected: ''
            }
        },

        methods: {
            toggle(e) {
                this.showDropdown = !this.showDropdown
            },
            changeCountry(item) {
                this.flagSelected = 'flag' + item.domain;
                this.codeSelected = '+' + item.code;
                this.domainSelected = item.domain.toUpperCase();
                this.showDropdown = false;
            },
            checkNumber(checkingNumber){
                this.showWarning = !(/^\d+$/.test(checkingNumber));
            },
            send(event){
                event.preventDefault();
                console.log(this);
                this.sendSuccesss = !this.sendSuccesss;
                this.sendMessage = 'Data has been updated';
                this.sendActive = !this.sendActive;
                // send get request
//                this.$http({url: '/admin/products/store', payload: payload, method: 'POST'}).then(
//                 function(response) {
                  // this.sendMessage = 'Data has been updated'; },
                // function(response) {
                  // this.sendMessage = 'Error. We are sorry'; });
            }
        }
    }
</script>



<style scoped>

    .form{
        padding: 15px;
    }

    .form__title{
        font-size: 14px;
        color: #cccccc;
        margin: 0 10px;
    }

    .tel {
        border-bottom: 1px #cccccc solid;
        padding-bottom: 10px;
        margin: 10px;

    }

    .button{
        background: none;
        border: none;
    }

    .button:focus {
        outline: 0;
    }

    .dropdown {
        position: relative;
        display: inline-block;
        cursor: pointer;
    }

    .dropdown.open .dropdown__button {
        color: #4594f4;
    }

    .dropdown__button {
        background: none;
        border: none;
        padding: 0;
        font-size: 13px;
        font-size: 0.8125rem;
        color: #888888;
        width: 100px;
        letter-spacing: 0.4px;
        -webkit-transition: all 0.12s linear;
        -moz-transition: all 0.12s linear;
        -o-transition: all 0.12s linear;
        transition: all 0.12s linear;
    }

    .dropdown__button:hover {
        color: #555555;
    }

    .open > .dropdown__menu {
        display: block;
    }

    .open .dropdown__arrow {
        transform: rotate(180deg);
        display: inline-block;
        text-align: right;
        vertical-align: middle;

    }

    .dropdown__arrow {
        -webkit-transition: all 0.2s linear;
        -moz-transition: all 0.2s linear;
        -o-transition: all 0.2s linear;
        transition: all 0.2s linear;
        margin-left: 2px;
        font-size: 8px;
        vertical-align: middle;
    }

    .flag {
        display: inline-block;
        width: 16px;
        height: 10px;
        background: url('/static/assets/flags.png') no-repeat;
        margin-right: 5px;
    }

    .flag.flagnl {
        background-position: -224px -90px;
    }

    .flag.flagby {
        background-position: -240px -10px;
    }

    .flag.flagth {
        background-position: -16px -130px;
    }

    .flag.flagvn {
        background-position: -112px -140px;
    }

    .flag.flagpl {
        background-position: -176px -100px;
    }

    .flag.flagempty {
        display: none;
    }

    .dropdown__menu {
        position: absolute;
        top: 100%;
        z-index: 1000;
        display: none;
        min-width: 200px;
        margin: 0;
        list-style: none;
        border-radius: 4px;
        padding: 0;
        box-shadow: 0px 0px 5px 0px #CBCBCB;
        line-height: 40px;
        background: #fff;
        height:160px;
        overflow-y: scroll;
    }

    .dropdown__item {
        display: block;
        padding: 0rem 1.25rem;
        cursor: pointer;
        white-space:nowrap;
        font-size: 0;
        height: 40px;
    }

    .dropdown__item__code{
        float:right;
    }

    .dropdown__menu li:hover {
        background: #f6f6f6;
    }

    .tel_number__wrapper{
        position: relative;
        display: inline-block;
    }

    .tel_number, .tel_number:focus{
        outline: none;
        box-shadow: none;
        border: none;
    }

    .tel_number__warning{
        position: absolute;
        display: none;
        z-index: 100;
        bottom: -5px;

        transform: translateY( 100%);
        text-align: center;
        vertical-align: middle;
        line-height: 30px;
        font-size: 14px;
        color:red;
        background: #fff;
        box-shadow: 0px 0px 5px 0px #CBCBCB;
        width: 150px;
        border-radius: 3px;
    }

    .tel_number__warning.tel_number__warningShow{
        display: block;
    }

    .tel_number__warning__triangle{
        position: absolute;
        color:white;
        font-size: 14px;
        top: -19px;
        left: 15%;
        text-shadow: 0px -2px 5px #CBCBCB;
    }

   .tel_number__warning__triangle{
       font-size: 14px;
    }

    span{
        display: inline-block;
        font-size: 16px;
    }

    .form-handle{
        display: flex;
        justify-content:space-between;
    }

    .form__message, .button__send{
        background: #70cd88;
        color: white;
        margin: 10px;
    }

    .form__message__wrapper{
        position: relative;
        overflow: hidden;
        width: 250px;;
    }


    .form__message{
        -webkit-transition: all 0.2s ease-in-out;
        -moz-transition: all 0.2s ease-in-out;
        -o-transition: all 0.2s ease-in-out;
        transition: all 0.2s ease-in-out;
        position: absolute;
        white-space: nowrap;
        top:0;
        left: 0%;
        transform: translatex(-110%);
        padding: 0 15px;
        font-size: 14px;
        border-top-right-radius:15px;
        border-bottom-right-radius:15px;
        line-height: 30px;
    }

    .form__messageOpen{
        transform: translatex(0);
    }

    .button__send{
        height: 30px;
        width: 40px;
        border-radius: 15px;
        cursor: pointer;
    }

    .button__send:disabled{
        cursor: default;
        opacity: 0.7;
    }

</style>