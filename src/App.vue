<template>
    <div id="app" class="container mt-5">
        <h1 class="mb-4">Бронирование мест в кинотеатре</h1>
        <div class="mb-3">
            <label for="date-picker" class="form-label">Выберите дату:</label>
            <input type="date" class="form-control"/>
        </div>

        <div class="mb-4 d-flex flex-wrap">
            <h3 class="w-100">Доступные сеансы:</h3>
            <div>
                <button class="btn btn-primary me-2">
                    10:00
                </button>
            </div>
        </div>

        <div class="mb-4">
            <h4>Места</h4>
            <div class="d-flex flex-wrap">
                <button class="btn btn-primary me-2">
                    A
                </button>
            </div>
        </div>

        <div>
            <p>Выберите дату и сеанс, чтобы просмотреть доступные места.</p>
        </div>
    </div>
</template>

<script>
</script>