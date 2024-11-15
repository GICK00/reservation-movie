<template>
    <div id="app" class="container mt-5">
        <h1 class="mb-4">Бронирование мест в кинотеатре</h1>
        <div class="mb-3">
            <label for="date-picker" class="form-label">Выберите дату:</label>
            <input type="date" id="date-picker" class="form-control" v-model="selectedDate" @change="updateSessions"
                :min="today" :max="maxBookingDate" />
        </div>

        <div v-if="sessions.length" class="mb-4 d-flex flex-wrap">
            <h3 class="w-100">Доступные сеансы:</h3>
            <div v-for="(session, index) in sessions" :key="index" class="mb-3">
                <button class="btn btn-primary me-2" :class="{ 'btn-secondary': isPastSession(session) }"
                    @click="selectSession(session)" :disabled="isPastSession(session)">
                    {{ session }}
                </button>
            </div>
        </div>

        <div v-if="selectedSession" class="mb-4">
            <h4>Места в {{ selectedDate }} - {{ selectedSession }}</h4>
            <div class="d-flex flex-wrap">
                <button v-for="seat in seats" :key="seat" class="btn me-2 mb-2" :class="{
                    'btn-success': !reservedSeats.includes(seat),
                    'btn-danger': reservedSeats.includes(seat)
                }" @click="reserveSeat(seat)"
                    :disabled="isPastSession(selectedSession) || reservedSeats.includes(seat)">
                    {{ seat }}
                </button>
            </div>
        </div>

        <div v-else>
            <p>Выберите дату и сеанс, чтобы просмотреть доступные места.</p>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            today: new Date().toISOString().split("T")[0],
            selectedDate: "",
            selectedSession: null,
            sessions: [],
            seats: ["A1", "A2", "A3", "B1", "B2", "B3", "C1", "C2", "C3"],
            reservedSeats: []
        };
    },
    computed: {
        maxBookingDate() {
            const date = new Date();
            date.setDate(date.getDate() + 7);
            return date.toISOString().split("T")[0];
        }
    },
    created() {
        this.loadReservations();
    },
    methods: {
        updateSessions() {
            this.selectedSession = null;
            this.sessions = [];
            const startDate = new Date(this.selectedDate);
            const startHour = 10;
            const endHour = 20;

            if (!isNaN(startDate)) {
                for (let hour = startHour; hour <= endHour; hour += 2) {
                    const sessionTime = `${hour.toString().padStart(2, "0")}:00`;
                    this.sessions.push(sessionTime);
                }
            }
        },
        selectSession(session) {
            this.selectedSession = session;
            this.loadReservations();
        },
        isPastSession(session) {
            const sessionDate = new Date(`${this.selectedDate}T${session}`);
            return sessionDate < new Date();
        },
        reserveSeat(seat) {
            if (!this.reservedSeats.includes(seat)) {
                this.reservedSeats.push(seat);
                this.saveReservations();
            }
        },
        loadReservations() {
            if (this.selectedDate && this.selectedSession) {
                const storedData = JSON.parse(localStorage.getItem("reservations") || "{}");
                const key = `${this.selectedDate}-${this.selectedSession}`;
                this.reservedSeats = storedData[key] || [];
            }
        },
        saveReservations() {
            if (this.selectedDate && this.selectedSession) {
                const storedData = JSON.parse(localStorage.getItem("reservations") || "{}");
                const key = `${this.selectedDate}-${this.selectedSession}`;
                storedData[key] = this.reservedSeats;
                localStorage.setItem("reservations", JSON.stringify(storedData));
            }
        }
    }
};
</script>
