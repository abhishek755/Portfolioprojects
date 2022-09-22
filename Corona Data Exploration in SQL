use Portfolioproject
select *
from coviddeaths
where continent is not null
order by 3,4

-- select *
-- from covidvaccinations
-- order by 3,4

-- Total cases vs Total Deaths and Death percent in Germany 

select location , date, total_cases, new_cases, total_deaths, population, (total_deaths/total_cases)*100 as deathpercent
from coviddeaths
where location = 'Germany' and total_cases is not null and continent is not null
order by 1,2 

-- Countries with Highest covid cases per population

select location , population, max(total_cases) as Highestcovidcase, round(max((total_cases/population))*100,2) as Highestcovidcasepercent
from coviddeaths
where population is not null and continent is not null
group by location, population
order by Highestcovidcasepercent desc

-- Countries with Highest covid Death per population

select location ,  max(cast(total_deaths as float)) as totaldeathcount
from coviddeaths
where population is not null and continent is not null
group by location
order by totaldeathcount desc

-- Deaths as per continent

select continent, max(cast(total_deaths as float)) as totaldeathcount
from coviddeaths
where population is not null and continent is not null
group by continent
order by totaldeathcount desc


-- Global ratio of death and cases

select sum(new_cases) as totalcases, sum(cast(new_deaths as float)) as totaldeaths, 
sum(cast(new_deaths as float))/SUM(new_cases)*100 as death_percent
from coviddeaths
where population is not null and continent is not null

-- joining death and vaccine table

select cd.continent, cd.location, cd.date, cd.population, cv.new_vaccinations,
sum(cast(cv.new_vaccinations as float))  over (partition by cv.location order by cd.location, cd.date) as sumnewvac
from coviddeaths as cd
join covidvaccinations as cv
	on cd.location = cv.location
	and cd.date = cv.date
where cd.continent is not null
order by location, date

-- use CTE
WITH popvsvac (continent, location, date, population, new_vaccinations, sumnewvac)
as 
(
select cd.continent, cd.location, cd.date, cd.population, cv.new_vaccinations,
sum(cast(cv.new_vaccinations as float))  over (partition by cv.location order by cd.location, cd.date) as sumnewvac
from coviddeaths as cd
join covidvaccinations as cv
	on cd.location = cv.location
	and cd.date = cv.date
where cd.continent is not null
-- order by location, date
)
select *, (sumnewvac/population)*100
from popvsvac

-- TEMP TABLE

create table percentpopulationvaccinated
(
continent nvarchar(255),
location nvarchar(255),
date datetime,
population float,
new_vaccinations float,
sumnewvac float
)

insert into percentpopulationvaccinated
select cd.continent, cd.location, cd.date, cd.population, cv.new_vaccinations,
sum(cast(cv.new_vaccinations as float))  over (partition by cv.location order by cd.location, cd.date) as sumnewvac
from coviddeaths as cd
join covidvaccinations as cv
	on cd.location = cv.location
	and cd.date = cv.date
where cd.continent is not null
-- order by location, date
select *, (sumnewvac/population)*100
from percentpopulationvaccinated

-- creating views for Data viz.

create view globaldeath as
select sum(new_cases) as totalcases, sum(cast(new_deaths as float)) as totaldeaths, 
sum(cast(new_deaths as float))/SUM(new_cases)*100 as death_percent
from coviddeaths
where population is not null and continent is not null












