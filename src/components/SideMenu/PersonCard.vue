<template>
    <div class="person">
        <div class="person__info--wrapper">
            <div class="person__photo">
                <img
                    :src="person.picture"
                    alt="photo"
                />
            </div>
            <div class="person__info">
                <div class="person__info-name">
                    <b>{{ person.name }}</b>
                </div>
                <div class="person__info-group">
                    <b>{{ person.group }}</b>
                </div>
            </div>
        </div>
        <div class="person__about">
            <div class="person__about-mail">
                <EmailSVG/> {{ person.email }}
            </div>
            <div class="person__about-mail">
                <HowToReg/> {{ formatedDate }}
            </div>
            <div class="person__about-info">
                <AboutSVG/> {{ person.about }}
            </div>
        </div>
    </div>
</template>

<script>
import { parseISO, format } from 'date-fns'
import EmailSVG from '@/assets/images/email_black_24dp.svg'
import AboutSVG from '@/assets/images/feed_black_24dp.svg'
import HowToReg from '@/assets/images/how_to_reg_black_24dp.svg'

export default {
    props: {
        person: {
            type: Object,
            default: null,
        },
    },
    components: {
        EmailSVG,
        AboutSVG,
        HowToReg
    },
    computed: {
        formatedDate() {
            const formatToISO = parseISO(this.person.registered)
            
            if (formatToISO) {
                return format(formatToISO, 'MM.dd.yyyy')
            } else {
                return ''
            }
        },
    }
};
</script>

<style scoped>
.person {
    display: grid;
}

.person__info--wrapper {
    display: grid;
    grid-template-columns: 100px 1fr;
    grid-gap: 16px;
    margin-bottom: 10px;
}

.person__photo img {
    height: 100px;
    width: 100px;
    border-radius: 5px;
}

.person__info-name {
    color: #7798f3;
    margin-bottom: 10px;
}

.person__info-group {
    color: #bbbdc3;
}

.person__about, .person__about-mail, .person__about-info{
    display: grid;
    grid-gap: 8px;
}

.person__about-mail, .person__about-info{
    grid-template-columns: 24px 1fr;
}

.person__about-mail {
    align-items: center;
}
</style>
