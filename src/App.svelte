<script>
    import { setupI18n } from './i18n';

    setupI18n({ withLocale: 'en' });

    import { _ } from 'svelte-i18n';
    import { compute_rest_props } from 'svelte/internal';

    /**
     * Returns classes for styling like "today", "tomorrow", or "future" when given a weekday index
     * @param {number} weekday - The index of the weekday (starting with monday = 1 ig)
     */
    function getCSSClassesForWeekday(weekday) {
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

    /**
     * Check if a given date is in the current week
     * @param date
     */
    function isDateInThisWeek(date) {
        const todayObj = new Date();
        const todayDate = todayObj.getDate();
        const todayDay = todayObj.getDay();

        // get first date of week
        const firstDayOfWeek = new Date(todayObj.setDate(todayDate - todayDay));

        // get last date of week
        const lastDayOfWeek = new Date(firstDayOfWeek);
        lastDayOfWeek.setDate(lastDayOfWeek.getDate() + 6);

        // if date is equal or within the first and last dates of the week
        return date >= firstDayOfWeek && date <= lastDayOfWeek;
    }

    /**
     * Returns a list of courses with their assignments filtered for the given weekday
     * @param {number} weekday
     */
    function getAssignmentsForDay(weekday) {
        let filteredCourses = [];

        for (let c of mockupData) {
            const filteredAssignments = c.assignments.filter((a) => {
                return (
                    isDateInThisWeek(a.dueDate) && a.dueDate.getDay() == weekday
                );
            });

            if (filteredAssignments.length > 0) {
                c.assignments = filteredAssignments;
                filteredCourses.push(c);
            }
        }

        console.log(filteredCourses);
        return filteredCourses;
    }

    export const days = [
        'monday',
        'tuesday',
        'wednesday',
        'thursday',
        'friday',
    ];

    const mockupData = [
        {
            id: 1,
            name: 'nen kurs',
            assignments: [
                { id: 'fsdaghks', dueDate: new Date(2021, 7, 17, 0, 0, 0) },
                { id: 'was anders', dueDate: new Date(2021, 7, 13, 0, 0, 0) },
                { id: 'gasdg', dueDate: new Date(2021, 7, 19, 0, 0, 0) },
                { id: 'fsdaghks', dueDate: new Date(2021, 7, 11, 0, 0, 0) },
            ],
        },
    ];
</script>

<div id="week-container">
    <h1>{$_('thisWeek')}</h1>
    <div id="week-row">
        {#each [1, 2, 3, 4, 5] as day}
            <div class="day">
                <h2 class={getCSSClassesForWeekday(day)}>
                    {$_(days[day - 1])}
                </h2>
                <div class="day-content">
                    {#each getAssignmentsForDay(day) as course}
                        <p>{course.name}</p>
                        {#each course.assignments as assignment}
                            {assignment.id}
                        {/each}
                    {/each}
                </div>
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

    .day-content {
        text-align: left !important;
        width: 100%;
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
