# EpsilonSecure
# Спецификация протокола Эпсилон для безопасной передачи данных
Протокол Эпсилон (на ходу придумал)

Мощный протокол позволяющий без передачи айпи адресов реализовать p2p соединение. (Магия!). Изначально предназначен для одноименного мессенджера

# Необходимые структуры:
1) Блокчейн сеть для регистрации узлов
2) Блокчейн сеть для регистрации профилей (в них будет указана информация, как довести сообщение)
3) p2p сеть из всех узлов

# Обозначения:
1) Пользователь - любой человек зашедший в сеть. Пользователь должен инициализировать хотя бы один узел
2) Узел - ПО для обеспечения onion-routing во всей сети. Принимает и передает пакеты.
3) Любой пользователь это узел но не любой узел это пользователь

# Добавление узла как передатчика в сеть:
1) Узел использует предыдущих/вшитых соседей для того чтобы провзаимодействовать с блокчейном. Блок в блокчейне - свзяка "ID + публичный ключ" 
2) После добавления узел обращается к своим помеченным соседям за получением рукопожатия через посредника (получения IP адреса соседа)
3) Как только узел узнает всех своих соседей он считается подключенным к сети и может с ней взаимодействовать

# Соседи:
Соседний узел это узел у которого айди больше или меньше на степень какого то числа D данного узла. То есть если D = 2 то соседи узла с ID 16 это 15; 17; 14; 18; 12; 20 и т.д.

# ID:
ID узла это постоянно изменяемое число которое условно означает адрес узла и позволяет ориентироваться между узлами. После каждого запуска узла ID скорее всего будет изменен

# Профиль:
Профиль это набор данных о пользователе (данные о маршруте, имя пользователя, ссылка но аватарку и тому подобное). Профили хранятся в отдельном блокчейне

# Маршрут:
Маршрут это рекурсивно зашифрованный объект (onion routing). Каждый слой содержит IP адрес следующего узла для прохождения до конечной цели. Конечный зашифрованный блок не содержит никакого сообщения (!). Слои должны ссылаться только на соседей. Иначе узлу придётся достраивать маршрут самостоятельно

# Пакет:
Пакет содержит зашифрованное сообщение и маршрут. Каждый узел передающий пакет расшифровывает маршрут на один слой но не трогает сообщение.

# Как передать любое сообщение любому пользователю без раскрытия чьих либо адресов?:
1) Ищем профиль адресата и находим там маршрут и добавляем наш маршрут до входного узла нашего собеседника.
2) Из того же профиля берем публичный ключ
3) Зашифровываем наше сообщение своим приватным ключом
4) Кладём с зашифрованным сообщением наш публичный ключ / юзернейм (для подтверждения личности)
5) Полученный блок передачи шифруем публичным ключом адресата
6) Объединяем сообщение с маршрутом
7) Отправляем на первый узел маршрута полученный пакет (его IP адрес мы знаем так как он наш сосед)
8) Отправка готова

Вот и весь протокол. Как вы понимайте чем больше D тем быстрее будет работать сеть (прыжки по узлам больше), однако слишком большим D тоже не должно быть т.к. будет замедлять работу алгоритма при маленьком расстоянии между узлами.
