select * from customer c where c.name like '%Eberwalde%'


select
 m.meta ->> 'serial' as "serial",
 m.meta ->> 'manufacturer' as "manufacturer",
  (
    select
      name
    from
      tree_node z
    where
      z.id = (
        select
          x.parent_tree_node_id
        from
          tree_node x
        where
          x.id = t.parent_tree_node_id
      )
  ) as "Object",
  CONCAT('https://gk4null.de/manager/', t.parent_tree_node_id, '/spots/', t.id) as "URL",
  s.id as "spot_id",
  s.health,
  c.name,
  m.device_address,
--  g.serial as "gateway_id",
  s."role",
  s.health_change_date
from
  spot s
  inner join tree_node t on s.id = t.id
  inner join device_mount m on s.id = m.spot_id
  inner join customer c on t.customer_id = c.id
--  inner join gateway g on g.id = m.gateway_id 
where
  t.type = 'Spot'
  and m.removal_date is null
 and t.customer_id = '55bbf838-58cb-40b5-b12b-2b7a82247f05'
order by
  c.name,s.health;
