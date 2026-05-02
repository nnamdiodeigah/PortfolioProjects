SELECT * 
FROM PortfolioProject..CovidDeaths
WHERE continent is not null
ORDER BY 3,4

SELECT * 
FROM PortfolioProject..CovidVaccinations
ORDER BY 3,4


SELECT Location, date, population, total_cases, new_cases, total_deaths
FROM PortfolioProject..CovidDeaths
WHERE continent is not null
ORDER BY 1,2


--Total Cases vs Total Deaths
--Shows Likelyhood of Death if Infected
SELECT Location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
FROM PortfolioProject..CovidDeaths
WHERE location like '%United Kingdom%'
ORDER BY 1,2


--Total Cases vs Population
--Percentage of Population Infected
SELECT Location, date, total_cases, population, (total_cases/population)*100 as PopulationInfectedPercentage
FROM PortfolioProject..CovidDeaths
WHERE location like '%United Kingdom%'
ORDER BY 1,2


--Countries with Highest infection rate compared to population
SELECT Location, population, MAX(total_cases) as HighestInfectionCount, MAX((total_cases/population))*100 as PopulationInfectedPercentage
FROM PortfolioProject..CovidDeaths
--WHERE location like '%United Kingdom%'
GROUP BY Location, population
ORDER BY PopulationInfectedPercentage desc


--Countries with Highest Death count per population
SELECT Location, MAX(cast(total_deaths as int)) as TotalDeathCount
FROM PortfolioProject..CovidDeaths
--WHERE location like '%United Kingdom%'
WHERE continent is not null
GROUP BY Location
ORDER BY TotalDeathCount desc


--BY CONTINENT
--Shows continents with hoghest deaths count per population
SELECT continent, MAX(cast(total_deaths as int)) as TotalDeathCount
FROM PortfolioProject..CovidDeaths
--WHERE location like '%United Kingdom%'
WHERE continent is not null
GROUP BY continent
ORDER BY TotalDeathCount desc


--Shows the numbers globally
SELECT SUM(new_cases) as total_cases, SUM(cast(new_deaths as int)) as total_deaths, SUM(cast(new_deaths as int))/SUM(new_cases)*100 as DeathPercentage
FROM PortfolioProject..CovidDeaths
where continent is not null
ORDER BY 1,2


--USE CTE
--Total Population vs Vaccinations
With PopvsVac (continent, location, date, population, new_vaccinations, RollingPeopleVaccinated)
as
(
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations 
, SUM(CONVERT(int,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.date) as RollingPeopleVaccinated
FROM PortfolioProject..CovidDeaths dea
Join PortfolioProject..CovidVaccinations vac
ON dea.location = vac.location
and dea.date = vac.date
Where dea.continent is not null
--ORDER BY 2,3
)
SELECT *, (RollingPeopleVaccinated/Population)*100 
FROM PopvsVac


--TEMP TABLE
DROP Table if exists #PercentPopulationVaccinated
CREATE TABLE #PercentPopulationVaccinated
(
Continent nvarchar(255),
Location nvarchar(255),
Date datetime,
Population float,
New_Vaccinations float,
RollingPeopleVaccinated float
)


Insert into #PercentPopulationVaccinated
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations 
, SUM(CONVERT(int,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.date) as RollingPeopleVaccinated
FROM PortfolioProject..CovidDeaths dea
Join PortfolioProject..CovidVaccinations vac
ON dea.location = vac.location
and dea.date = vac.date
Where dea.continent is not null
--ORDER BY 2,3

SELECT *, (RollingPeopleVaccinated/Population)*100
FROM #PercentPopulationVaccinated



--Creating a view to store data for data visualization
CREATE View PercentPopulationVaccinated as
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations 
, SUM(CONVERT(int,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.date) as RollingPeopleVaccinated
FROM PortfolioProject..CovidDeaths dea
Join PortfolioProject..CovidVaccinations vac
	ON dea.location = vac.location
	and dea.date = vac.date
Where dea.continent is not null
--ORDER BY 2,3

SELECT *
FROM PercentPopulationVaccinated
