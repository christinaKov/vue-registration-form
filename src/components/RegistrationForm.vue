<template>
    <div class="registration-form">
            <p class="title">Регистрация</p>
            <p class="log-in">Уже есть аккаунт?<span>Войти</span></p>
        
        <form @keydown="submitKey">
            <div class="input">
                <label for="name">Имя</label>
                <input @change="checkInputValidation" v-model="form.name" id="name" type="text" placeholder="Введите Ваше имя">
                <p :class="{active: !validInputs.name}" class="invalid-message">Введено не корректное значение</p>
            </div>
            <div class="input">
                <label for="email">Еmail</label>
                <input @change="checkInputValidation" v-model="form.email" id="email" type="text" placeholder="Введите ваш email">
                <p :class="{active: !validInputs.email}" class="invalid-message">Введено не корректное значение</p>
            </div>
            <div class="input">
                <label for="phone-number">Номер телефона</label>
                <input @change="checkInputValidation" v-model="form.phoneNumber" id="phone-number" type="text" placeholder="Введите номер телефона">
                <p :class="{active: !validInputs.phoneNumber}" class="invalid-message">Введено не корректное значение</p>
            </div>

            <select id='language-select' v-model="form.language" name="language">
                <option value="Русский">Русский</option>
                <option value="Английский">Английский</option>
                <option value="Китайский">Китайский</option>
                <option value="Испанский">Испанский</option>
            </select>

            <div class="input">
                <p class="language-label">Язык</p>
                <p @click="languageSelect" @mousedown="preventTextSelection" :class="{active: languageSelectActive}" class="language-select-btn">{{form.language}}<img src='../icons/arrow.svg'></p>
                <div class="language-options" :class="{active: languageSelectActive}">
                    <p @click="changeLanguage" class="language-option">Русский</p>
                    <p @click="changeLanguage" class="language-option">Английский</p>
                    <p @click="changeLanguage" class="language-option">Китайский</p>
                    <p @click="changeLanguage" class="language-option">Испанский</p>
                </div>
            </div>

            <div class="checkbox-container">
                <div @mousedown="preventTextSelection" @click="agreeToTermsOfUse" :class="{checked: form.termsOfUse}" class="checkbox"><img src="../icons/check-mark.svg" alt=""></div>
                <p>Принимаю <span>условия</span> использования</p>
            </div>

            <input id="terms-checkbox" v-model="form.termsOfUse" type="checkbox">

            <button :class="{active: formIsValid}" type="submit">Зарегистрироваться</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'RegistrationForm',
    data() {
        return {
            form: {
                name: '',
                email: '',
                phoneNumber: '',
                language: 'Язык',
                termsOfUse: false
            },
            validInputs: {
                name: true,
                email: true,
                phoneNumber: true
            },
            languageSelectActive: false,
            formIsValid: false
        }
    },
    methods: {
        submitKey(e) {
            if (e.keyCode == 13) {
                if (!this.formIsValid) {
                    e.preventDefault();
                    return false;
                }
            }
        },
        languageSelect() {
            this.languageSelectActive = !this.languageSelectActive;
        },
        preventTextSelection(e) {
            e.preventDefault();
        },
        changeLanguage(e) {
            this.form.language = e.target.innerText;
            this.languageSelectActive = false;
        },
        agreeToTermsOfUse() {
            this.form.termsOfUse = !this.form.termsOfUse;
            this.checkFormValidation();
        },
        checkInputValidation(e) {
            switch (e.target.id) {
                case 'name':
                    this.validInputs.name = /^[a-zA-Z\u0400-\u04FF\s-]+$/.test(this.form.name);
                    break;
                case 'email':
                    this.validInputs.email = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/.test(this.form.email);
                    break;
                case 'phone-number':
                    this.validInputs.phoneNumber = /^[+]?[0-9]{1}\(?([0-9]{3})\)?(-)?([0-9]{3})\2([0-9]{4})$/.test(this.form.phoneNumber);
            }
            this.checkFormValidation();
        },
        checkFormValidation() {
            if (this.validInputs.name && this.validInputs.email && this.validInputs.phoneNumber && this.form.termsOfUse) {
                this.formIsValid = true;
            } else {
                this.formIsValid = false;
            }
        }
    }
}
</script>

<style scoped>
    .registration-form {
        margin: 40px 30px;
    }

    .title {
        font-weight: 700;
        font-size: 34px;
        margin: 0;
        margin-bottom: 8px;
        color: #2C2738;
    }

    .log-in {
        margin: 0;
        color: #2C2738;
        margin-bottom: 58px;
        line-height: 22px;
    }

    .log-in span {
        margin-left: 6px;
        font-size: 16px;
        line-height: 22px;
    }

    span {
        cursor: pointer;
        color: #0880AE;
    }

    .input {
        display: flex;
        flex-direction: column;
        margin-bottom: 34px;
        min-width: 300px;
        width: calc(100vw - 60px);
    }

    label,
    .language-label {
        font-size: 16px;
        font-weight: 500;
        color: #756F86;
        margin-bottom: 7px;
        margin-top: 0;
        line-height: 21px;
    }

    input {
        font-size: 16px;
        line-height: 21px;
        color: #2C2738;
        font-family: 'IBM Plex Sans', sans-serif;
    }

    input:focus {
        border: 2px solid #0880AE;
        box-sizing: border-box;
    }

    input::placeholder,
    .language-select-btn {
        font-size: 16px;
        line-height: 21px;
        font-weight: 400;
        font-family: 'IBM Plex Sans', sans-serif;
        color: #7C9CBF;
    }

    .language-select-btn.active {
        border: 2px solid #0880AE;
        box-sizing: border-box;
        color: #2C2738;
    }

    input,
    .language-select-btn,
    .language-options {
        padding: 16px;
        border: 1px solid #DBE2EA;
        box-sizing: border-box;
        box-shadow: 0px 4px 8px rgba(44, 39, 56, 0.04);
        border-radius: 6px;
        height: 52px;
        outline: none;
    }

    .language-select-btn {
        cursor: pointer;
        display: flex;
        justify-content: space-between;
        margin: 0;
    }

    .language-options {
        display: none;
        padding: 12px 0;
        margin-top: 84px;
        height: 200px;
        position: absolute;
        min-width: 300px;
        width: calc(100vw - 60px);
        background: white;
        box-sizing: border-box;
    }

    .language-options.active {
        display: block;
    }

    .language-option {
        margin: 0;
        padding: 12px 16px;
        box-sizing: border-box;
        cursor: pointer;
    }

    .language-option:hover {
        background: #EBF4F8;
    }

    .checkbox-container {
        margin-bottom: 37px;
        display: flex;
        align-items: center;
    }

    .checkbox {
        border-radius: 4px;
        width: 28px;
        height: 28px;
        border: 1px solid #DBE2EA;
        box-sizing: border-box;
        box-shadow: 0px 4px 8px rgba(44, 39, 56, 0.04);
        margin-right: 8px;
        cursor: pointer;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .checkbox.checked {
        border: 2px solid #0880AE;
        box-sizing: border-box;
    }

    .checkbox img {
        display: none;
    }

    .checkbox.checked img {
        display: block;
    }

    .checkbox-container p { 
        margin: 0;
        font-weight: 500;
        font-size: 16px;
        line-height: 21px;
    }

    .invalid-message {
        font-size: 14px;
        color: transparent;
        position: absolute;
        margin: 0;
        display: none;
        margin-top: 88px;
    }

    .invalid-message.active {
        color: #FF7171;
        display: block;
    }

    button {
        margin: 0;
        width: 100%;
        height: 56px;
        color: #B1B5BF;
        background: #DBE2EA;
        box-shadow: 0px 2px 4px rgba(44, 39, 56, 0.08), 0px 4px 8px rgba(44, 39, 56, 0.08);
        border-radius: 6px;
        border: none;
        font-size: 1rem;
        font-weight: 500;
        font-family: 'IBM Plex Sans', sans-serif;
        pointer-events: none;
        outline: none;
    }

    button:active {
        border: 2px solid rgba(44, 39, 56, 0.86);
        box-sizing: border-box;
    }

    button.active {
        color: white;
        background: #0880AE;
        cursor: pointer;
        pointer-events: all;
    }

    #language-select,
    #terms-checkbox {
        display: none;
    }
</style>