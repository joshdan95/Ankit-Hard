# Ankit-Hard
1.Input
![image](https://github.com/user-attachments/assets/e654983d-7fec-4eba-8926-a1e2445bda9e)

output

![image](https://github.com/user-attachments/assets/77c88fbc-02b0-45a0-b9fe-283006b7de03)


select Team_name, count(1) as no_of_matches_played, sum(win_flag) as No_of_wins, count(1) - sum(win_flag) as No_of_losses
from
(select team_1 as Team_name,
case
when team_1 = winner then 1 else 0
end as win_flag from icc_world_cup
union all
select team_2 as Team_name,
case when team_2 = winner then 1 else 0
end as win_flag_2
from icc_world_cup) as A
group by team_name
order by sum(win_flag) desc;

or another method 

with cte1 as (
select team_1 as Team_name,
case when team_1 = winner then 1 else 0 end as win_flag,
case when team_1 <> winner and winner <> 'draw'  then 1 else 0 end as loss_flag, /////* what's the diff between team_1 <> draw and winner <> draw*/////
case when winner = 'draw' then 1 else 0 end as draw  //// why winner = draw instead of team_1 = draw///////
from icc_world_cup
union all
select team_2 as Team_name,
case when team_2 = winner then 1 else 0 end as win_flag_,
case when team_2 <> winner  and winner <> 'draw' then 1 else 0 end as loss_flag,
case when winner = 'draw' then 1 else 0 end as draw 
from icc_world_cup)

select team_name, count(1) as matches_played,sum(win_flag) as No_of_wins, sum(loss_flag) as no_of_losses, sum(draw) as draw
from cte1
group by team_name
order by sum(win_flag) desc;

--------------------------------------------------------------------------
