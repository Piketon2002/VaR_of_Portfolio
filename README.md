# Суть задачи

&ensp; &ensp; С помощью garch-моделей провести моделирование поведения портфеля из 5 акций с весами, полученными с помощью: 

* максимизации отношения мат. ожидания доходности в правом хвосте к мат. ожиданию доходности в левом хвосте (Expected Shortfall);
* минимизации VaR портфеля;
* максимизации ожидаемой доходности портфеля.

&ensp; &ensp; Посмотреть как ведет себя стоимость портфеля в различные периоды и как она соотносится со стоимостью портфеля с равными весами (по 20%).

&ensp; &ensp; Для моделирования структуры зависимости между доходностями акций использовались различные копулы: Нормальная, Гауссова, Клейтона, Стьюдента, Франка, Гумбеля.


# Используемые данные

&ensp; &ensp; С помощью `yfinance` были скачаны общедоступные котировки акций компаний Apple, Microsoft, Amazon, Netflix и Tesla за период с 2017 по 2022 года

# Стек технологий

`Python`, `Numpy`, `Pandas`, `SciPy` `Matplotlib`, `Seaborn`,`Plotly`,
 `yfinance`,
`arch`, `copulae`

# Метрики

&ensp; &ensp; Оценка качества полученных моделей производится с помощью статистического теста Купика, который сравнивает эмпирическую и модельную частоту превышений фактических убытков границы VaR (Желаемый уровень пробитий VaR использовался равным 5%).

# Результаты

&ensp; &ensp; При подборе копулы при фиксированных других параметрах во всех случаях уровень пробитий в модели статистически отличался от желаемого уровня пробитий, однако если сравнивать результаты друг с другом, то получилось, что при использовании `копулы Гумбеля` результат наиболее близок к желаемому (чуть больше 2% при желаемом 5%). 

&ensp; &ensp; После этого при использовании копулы Гумбеля подбиралось количество лагов garch - модели:

* p - порядок авторегрессии: количество лагов, используемых для моделирования дисперсии; 
* q - порядок скользящего среднего: количество лагов, используемых для моделирования дисперсии ошибки;  
* o - порядок интеграции: количество раз, которое нужно взять разности между текущим значением и предыдущими значениями, чтобы получить стационарный временной ряд.

&ensp; &ensp; В результате подбора этих параметров наилучшее качество модели получилось при следующих значениях: `p = 5, q = 2, o = 0`.

&ensp; &ensp; Далее при использовании этой модели оптимизировались веса портфеля (методами, указанными в "сути задачи"). В результате был построен график изменения стоимости портфеля со временем для рассмотренных стратегий. Если сравнивать поведение четырех портфелей - с константными весами, и с весами, которые максимизируют Expected Success/Shortfall Ratio, минимизируют VaR, максимизируют Expected return, то:

```
 До января 2020 года значения примерно одинаковые, однако далее становится очевидным то, что портфель 
 с константными весами (по 20%) начинает значительно проигрывать остальным (до 100% и более). Способ 
 максимизации отношения Expected Success/Shortfall Ratio оказался немного успешнее, чем минимизации 
 VaR портфеля, но несколько "хуже", чем метод максимизации Expected Return (который, естественно, более 
 рискованный, т.к. он рассчитывает лишь матожидание доходности портфеля и не учитывает никакие метрики 
 возможных потерь).
 ```
