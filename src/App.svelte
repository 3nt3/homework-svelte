<script>
    import { setupI18n } from './i18n';

    setupI18n({ withLocale: 'en' });

    function getClassesForWeekday(weekday) {
        const today = new Date().getDay();

        if (weekday === today) {
            return 'today';
        } else if (weekday === today + 1) {
            return 'tomorrow';
        } else if (weekday === today + 2) {
            return 'tdat';
        } else if (weekday < today) {
            return 'past';
        } else {
            return 'future';
        }
    }

    export const days = [
        'monday',
        'tuesday',
        'wednesday',
        'thursday',
        'friday',
    ];

    import { _ } from 'svelte-i18n';
</script>

<div id="week-container">
    <h1>{$_('next3Days')}</h1>
    <div id="week-row">
        {#each [1, 2, 3, 4, 5] as day}
            <div class="day">
                <h2 class={getClassesForWeekday(day)}>{$_(days[day - 1])}</h2>
            </div>
        {/each}
    </div>
</div>

<style>
    h1 {
        font-size: 22pt;
    }

    h2 {
        font-size: 14pt;
    }

    #week-row {
        display: flex;
        /* TODO: responsiveness */
    }

    #week-row div {
        width: 100%;
        /* color: white; */
        text-align: center;
    }

    .day h2 {
        text-decoration: underline;
        text-decoration-thickness: 4pt;
        text-decoration-color: #d1ccc0;
        text-underline-offset: 6px;
    }
    .today {
        text-decoration-color: #ff5252 !important;
    }
    .tomorrow {
        text-decoration-color: #ff793f !important;
    }

    .tdat {
        text-decoration-color: #ffb142 !important;
    }

    .future {
        text-decoration-color: #40407a !important;
    }
</style>
